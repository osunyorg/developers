---
title: Version 1
description: Implémentée en 2022
---

Nous avons procédé à un traitement à la volée des dépendances dans tous les objets. Cette version, détaillée ici, tombe dans les boucles infinies et est pénible à maintenir.


## Communication::Website
```ruby
def git_dependencies(website)
  dependencies = [
    self,
    config_default_languages,
    config_default_permalinks,
    config_development_config,
    config_production_config
  ] + menus
  dependencies += pages + pages.includes(parent: { featured_image_attachment: :blob }, featured_image_attachment: :blob).map(&:active_storage_blobs).flatten + pages.map(&:git_block_dependencies).flatten
  dependencies += posts + posts.includes(featured_image_attachment: :blob).map(&:active_storage_blobs).flatten + posts.map(&:git_block_dependencies).flatten
  dependencies += people_with_facets + people.map(&:active_storage_blobs).flatten + people.map(&:git_block_dependencies).flatten
  dependencies += organizations_in_blocks + organizations_in_blocks.map(&:active_storage_blobs).flatten + organizations_in_blocks.map(&:git_block_dependencies).flatten
  dependencies += categories + categories.map(&:git_block_dependencies).flatten
  dependencies += about.git_dependencies(website) if about.present?
  dependencies
end
```

## Communication::Block
```ruby
def git_dependencies
  template.git_dependencies
end
```

## Communication::Block::Component::Category
```ruby
def git_dependencies
  [category, category&.best_featured_image&.blob]
end
```

## Communication::Block::Component::File
```ruby
def git_dependencies
  [blob]
end
```

## Communication::Block::Component::Organization
```ruby
def git_dependencies
  [
    organization,
    organization&.logo&.blob,
    organization&.logo_on_dark_background&.blob
  ]
end
```

## Communication::Block::Component::Page
```ruby
def git_dependencies
  [page, page&.best_featured_image&.blob]
end
```

## Communication::Block::Component::Person
```ruby
def git_dependencies
  [person, person&.picture&.blob]
end
```

## Communication::Block::Component::Post
```ruby
def git_dependencies
  [
    post,
    post&.author,
    post&.author&.author,
    post&.author&.picture&.blob
  ]
end
```

## Communication::Block::Component::Program
```ruby
def git_dependencies
  [program, program&.best_featured_image&.blob]
end
```

## Communication::Block::Template::Base
```ruby
  def git_dependencies
    unless @git_dependencies
      @git_dependencies = []
      components.each do |component|
        add_dependency component.git_dependencies
      end
      elements.each do |element|
        add_dependency element.git_dependencies
      end
      add_custom_git_dependencies
      @git_dependencies.compact!
      @git_dependencies.uniq!
    end
    @git_dependencies
  end

  def active_storage_blobs
    @active_storage_blobs ||= git_dependencies.select { |dependency|
      # dependency.is_a? ActiveStorage::Blob misses some occurrences
      dependency.class.name == 'ActiveStorage::Blob'
    }.uniq
  end

  protected

  def add_custom_git_dependencies
  end
```

## Communication::Block::Template::Page
```ruby
  def add_custom_git_dependencies
    selected_pages.each do |page|
      add_dependency page
      add_dependency page.active_storage_blobs.to_a
    end
  end
```

## Communication::Block::Template::Post
```ruby
  def add_custom_git_dependencies
    selected_posts.each do |post|
      add_dependency post
      add_dependency post.active_storage_blobs.to_a
      if post.author.present?
        add_dependency [post.author, post.author.author]
        add_dependency post.author.active_storage_blobs.to_a
      end
    end
  end
```

## Communication::Website::Category
```ruby
  def git_dependencies(website)
    [self, parent].compact + siblings + descendants + active_storage_blobs + posts + website.menus
  end

  def git_destroy_dependencies(website)
    [self] + descendants + active_storage_blobs
  end
```

## Communication::Website::Page
```ruby
  def git_dependencies(website)
    dependencies = [self] +
                    website.menus +
                    descendants +
                    active_storage_blobs +
                    siblings +
                    git_block_dependencies +
                    type_git_dependencies
    dependencies += [parent] if has_parent?
    dependencies.flatten.compact
  end

  def git_destroy_dependencies(website)
    [self] +
    descendants +
    active_storage_blobs
  end
```

## Communication::Website::Post
```ruby
  def git_dependencies(website)
    dependencies = [self] + website.menus
    dependencies += categories
    dependencies += active_storage_blobs
    dependencies += git_block_dependencies
    dependencies += university.communication_blocks.where(template_kind: :posts).includes(:about).map(&:about).uniq
    if author.present?
      dependencies += [author, author.author, translated_author, translated_author.author]
      dependencies += author.active_storage_blobs
      dependencies += translated_author.active_storage_blobs
    end
    dependencies
  end

  def git_destroy_dependencies(website)
    [self] + explicit_active_storage_blobs
  end
```

## Communication::Website::WithDependencies
```ruby
module Communication::Website::WithDependencies
  extend ActiveSupport::Concern

  included do

    has_many    :pages,
                foreign_key: :communication_website_id,
                dependent: :destroy

    has_many    :posts,
                foreign_key: :communication_website_id,
                dependent: :destroy

    has_many    :authors, -> { distinct }, through: :posts

    has_many    :categories,
                class_name: 'Communication::Website::Category',
                foreign_key: :communication_website_id,
                dependent: :destroy

    has_many    :permalinks,
                class_name: "Communication::Website::Permalink",
                dependent: :destroy

  end

  def blocks
    @blocks ||= begin
      blocks = Communication::Block.where(about_type: 'Communication::Website::Page', about_id: pages)
      blocks = blocks.or(Communication::Block.where(about_type: 'Communication::Website::Post', about_id: posts))
      blocks = blocks.or(Communication::Block.where(about_type: 'Education::Program', about_id: education_programs)) if has_education_programs?
      blocks = blocks.or(Communication::Block.where(about_type: 'Education::Diploma', about_id: education_diplomas)) if has_education_diplomas?
      # TODO: Blocks from People & Organizations ?
      blocks
    end
  end

  def blocks_dependencies
    @blocks_dependencies ||= blocks.collect(&:git_dependencies).flatten.compact.uniq
  end

  def education_diplomas
    has_education_diplomas? ? about.diplomas : Education::Diploma.none
  end

  def education_programs
    has_education_programs? ? about.published_programs : Education::Program.none
  end

  def research_volumes
    has_research_volumes? ? about.published_volumes : Research::Journal::Volume.none
  end

  def research_papers
    has_research_papers? ? about.published_papers : Research::Journal::Paper.none
  end

  def administrators
    about&.administrators
  end

  def researchers
    about&.researchers
  end

  def teachers
    about&.teachers
  end

  def people_in_blocks
    @people_in_blocks ||= blocks_dependencies.reject { |dependency| !dependency.is_a? University::Person }
  end

  def organizations
    organizations_in_blocks
  end

  def organizations_in_blocks
    @organizations_in_blocks ||= blocks_dependencies.reject { |dependency| !dependency.is_a? University::Organization }
  end

  def people_with_facets_in_blocks
    @people_with_facets_in_blocks ||= blocks_dependencies.reject { |dependency| !dependency.class.to_s.start_with?('University::Person') }
  end

  def people
    # TODO: Scoper aux langues du website dans le cas où une personne serait traduite dans + de langues
    @people ||= begin
      people = []
      people += authors if has_authors?
      people += teachers if has_teachers?
      people += administrators if has_administrators?
      people += researchers if has_researchers?
      people += people_in_blocks if has_people_in_blocks?
      people.uniq.compact
    end
  end

  def people_with_facets
    # TODO: Scoper aux langues du website dans le cas où une personne serait traduite dans + de langues
    @people_with_facets ||= begin
      people_with_facets = people
      people_with_facets += authors.compact.map(&:author) if has_authors?
      people_with_facets += teachers.compact.map(&:teacher) if has_teachers?
      people_with_facets += administrators.compact.map(&:administrator) if has_administrators?
      people_with_facets += researchers.compact.map(&:researcher) if has_researchers?
      people_with_facets += people_with_facets_in_blocks if has_people_in_blocks?
      people_with_facets.uniq.compact
    end
  end

  # Deprecated, needs refactor for performance

  def has_communication_posts?
    posts.published.any?
  end

  def has_communication_categories?
    categories.any?
  end

  def has_organizations?
    has_organizations_in_blocks?
  end

  def has_authors?
    authors.compact.any?
  end

  def has_people_in_blocks?
    people_in_blocks.compact.any?
  end

  def has_organizations_in_blocks?
    organizations_in_blocks.compact.any?
  end

  def has_persons?
    has_authors? || has_administrators? || has_researchers? || has_teachers? || has_people_in_blocks?
  end

  def has_administrators?
    about && about.has_administrators?
  end

  def has_researchers?
    about && about.has_researchers?
  end

  def has_teachers?
    about && about.has_teachers?
  end

  def has_education_diplomas?
    about && about.has_education_diplomas?
  end

  def has_education_programs?
    about && about.has_education_programs?
  end

  def has_research_papers?
    about && about.has_research_papers?
  end

  def has_research_volumes?
    about && about.has_research_volumes?
  end

end
```

## Communication::Website::Page::Administrator
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website&.administrators&.map(&:administrator)
    ]
  end
```

## Communication::Website::Page::Author
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website&.authors&.map(&:author)
    ]
  end
```

## Communication::Website::Page::CommunicationPost
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website.categories,
      website.authors.map(&:author),
      website.posts
    ]
  end
```

## Communication::Website::Page::EducationDiploma
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website.education_diplomas
    ]
  end
```

## Communication::Website::Page::EducationProgram
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website.education_programs
    ]
  end
```

## Communication::Website::Page::Organization
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website.organizations
    ]
  end
```

## Communication::Website::Page::Person
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website.people_with_facets
    ]
  end
```

## Communication::Website::Page::ResearchPaper
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website.research_papers
    ]
  end
```

## Communication::Website::Page::ResearchVolume
```ruby
  def type_git_dependencies
    [
      website.config_default_permalinks,
      website.research_volumes
    ]
  end
```

## Communication::Website::Page::Researcher
```ruby
def type_git_dependencies
  [
    website.config_default_permalinks,
    website&.researchers&.map(&:researcher)
  ]
end
```

## Communication::Website::Page::Teacher
```ruby
def type_git_dependencies
  [
    website.config_default_permalinks,
    website&.teachers&.map(&:teacher)
  ].flatten
end
```

## module WithBlocks
```ruby
def git_block_dependencies
  blocks.collect &:git_dependencies
end
```

## Education::Diploma
```ruby
def git_dependencies(website)
  website_programs = programs_for_website(website)

  dependencies = [self]
  dependencies += website_programs + website_programs.map(&:active_storage_blobs).flatten
  dependencies
end
```

## Education::Program
```ruby
def git_dependencies(website)
  [self] +
  siblings +
  descendants +
  active_storage_blobs +
  git_block_dependencies +
  university_people_through_involvements +
  university_people_through_involvements.map(&:active_storage_blobs).flatten +
  university_people_through_involvements.map(&:teacher) +
  university_people_through_role_involvements +
  university_people_through_role_involvements.map(&:active_storage_blobs).flatten +
  university_people_through_role_involvements.map(&:administrator) +
  website.menus +
  [diploma]
end

def git_destroy_dependencies(website)
  [self] +
  explicit_active_storage_blobs
end
```

## Education::School
```ruby
def git_dependencies(website)
  dependencies = [self]
  dependencies += programs +
                  programs.map(&:active_storage_blobs).flatten +
                  programs.map(&:git_block_dependencies).flatten
  dependencies += diplomas +
                  diplomas.map(&:git_block_dependencies).flatten
  dependencies += teachers +
                  teachers.map(&:teacher) +
                  teachers.map(&:active_storage_blobs).flatten
  dependencies += researchers +
                  researchers.map(&:researcher) +
                  researchers.map(&:active_storage_blobs).flatten
  dependencies += administrators +
                  administrators.map(&:administrator) +
                  administrators.map(&:active_storage_blobs).flatten
  dependencies
end
```

## Research::Journal
```ruby
def git_dependencies(website)
  dependencies = [self]
  dependencies += papers + papers.map(&:active_storage_blobs).flatten
  dependencies += volumes + volumes.map(&:active_storage_blobs).flatten
  dependencies += researchers + researchers.map(&:researcher) + researchers.map(&:active_storage_blobs).flatten
  dependencies
end

def git_destroy_dependencies(website)
  [self] + papers + volumes
end
```

## Research::Journal::Paper
```ruby
def git_dependencies(website)
  [self] +
  active_storage_blobs +
  other_papers_in_the_volume +
  people +
  people.map(&:active_storage_blobs).flatten +
  people.map(&:researcher) +
  website.menus
end
```

## Research::Journal::Volume
```ruby
def git_dependencies(website)
  [self] +
  papers +
  people +
  people.map(&:active_storage_blobs).flatten +
  people.map(&:researcher) +
  active_storage_blobs +
  website.menus
end

def git_destroy_dependencies(website)
  [self] + active_storage_blobs
end
```

## University::Organization
```ruby
def git_dependencies(website)
  dependencies = []
  if for_website?(website)
    dependencies << self
    dependencies.concat active_storage_blobs
    dependencies.concat git_block_dependencies
  end
  dependencies += website.menus.to_a
  dependencies += dependencies_through_blocks(website)
  dependencies
end
```

## University::Person
```ruby
def git_dependencies(website)
  dependencies = [self]
  dependencies += active_storage_blobs
  dependencies += git_block_dependencies
  dependencies += [administrator, author, researcher, teacher]
  dependencies += communication_website_posts.where(communication_website_id: website.id)
  dependencies += website.menus.to_a
  dependencies += dependencies_through_blocks(website)
  dependencies
end
```
