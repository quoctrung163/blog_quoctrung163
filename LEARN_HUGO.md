### Learn Hugo

### Leson 1: Basic

- install hugo:
  
  - macOS: brew install hugo

- create new project
  
  - hugo new site

- install theme
  
  - clone repo theme on github or download somewhere
  - and copy to themes folder
  - and edit config.toml
  - for example: theme = "theme-name"

- to start pj
  
  - hugo server

### Lesson 2: Creating & Organizing Content

- create new file in content folder
  
  - hugo new a.md

- to start pj with draft file
  
  - hugo server -D 

- default hugo take folder lever1 in content for a new directory website and not take folder lever2:
  
  - ex1: localhost/dir1 (good)
  - ex2: localhost/dir2 (not display)
  - -> to fix:
    - hugo new dir1/dir2/_index.md

### Lesson 3: Front Matter

- edit mardown file
  - author: quoctrung163
  - change date, title, etc

### Lesson 4: Archetypes

- is a template file for create file in folder content
- default.md is a default in template file
- if you want to create new template -> create new file in folder archetypes 
  - ex: cd archetypes and touch dir1.md
  - copy default file to dir1.md and edit
  - to using template dir1
    - type: hugo new dir1/c.md

### Lesson 5: Shortcodes

- are a mean to consolidate templating into small, reusable snippets that you embed directly inside of your content.

- syntax: {{< shortcode-name parameter >}}

- for example: {{< youtube 2xkNJL4gJ9E >}}

### Lesson 6: Taxonomies (Phan loai)