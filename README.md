# blog_app

Création du projet blog en mode tuto
explication
On reprend, l'éternel, exercice du blog un de plus... :blush:
Ce projet est à disposition des apprenants afin de visualiser étape par étape la construction du blog
Chaque grande étape fera l'object d'une branche.
À l'issue de chaque branche, celle-ci sera merge dans develop
readme avancera en même temps que les branches, il comprendra les commandes à effectuer et les modifications de fichier.
Installation
Stack :
    • ruby 2.6.4
    • rails 6.0.x
    • sqlite3
Après avoir clone le projet, il vous faut renommer le fichier database.yml.sample en database.yml
rappel pour créer l'app :
$ rails new blog_howto
  create
  create  README.md
  create  Rakefile
  create  .ruby-version
  create  config.ru
  create  .gitignore
  create  Gemfile
  [...]
  yarn install
  [...]
  ✨  Done in 4.87s.
Lancement
$ rails db:create
Created database 'db/development.sqlite3'
Created database 'db/test.sqlite3'
C'est partie pour les features
users feature/users
Création de toute la structure pour gérer les user, le scaffold permet de créer :
    • la migration db/migrate/yyyymmjjhhmmss_create_users.rb
    • le controller app/controllers/users_controller.rb
    • le model app/model/user.rb
    • les vues app/views/users/*.html.erb
    • les fichiers de tests (et oui, on n'en parle pas maintenant, mais il va fallloir y aller)
    • les assets
    • le helper app/helpers/users_helper.rb
    • les routes config/routes.rb
$ rails g scaffold user name:string email:string:uniq:index
or
$ rails generate scaffold user name:string email:string:uniq:index
On peut lancer la migration
$ rails db:migrate
== 20200407064648 CreateUsers: migrating ======================================
-- create_table(:users)
   -> 0.0022s
-- add_index(:users, :email, {:unique=>true})
   -> 0.0010s
== 20200407064648 CreateUsers: migrated (0.0033s) =============================
installation de Faker pour remplir notre base de données de fausses datas
On édite le fichier Gemfile 🔗 et on y ajoute la gem, pas d'obligation de préciser la version, il prendra celle qu'il peut dans les plus récentes, en prenant en compte, les déprendences des autres gems. (bon, des fois, faut le forcer quand même)
group :development do
  # Access an interactive console on exception pages or by calling 'console' anywhere in the code.
  gem 'faker' 
  gem 'web-console', '>= 3.3.0' 
  gem 'listen', '>= 3.0.5', '< 3.2'
  # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
  gem 'spring'
  gem 'spring-watcher-listen', '~> 2.0.0'
end
Edition du fichier seed
Le fichier seeds.db nous permet de lancer des commandes ruby pour remplir ou tout au moins interagir avec la bdd. Dans notre cas nous allons l'utiliser pour mettre de la sample datacomme on peut l'appeller sur d'autres projets.
# delete all entries for moment, i don't use environnement
User.delete_all if User.all.size.positive?

# create 10 users
(1..10).each do |_number|
  User.create name:   Faker::Name.name,
              email:  Faker::Internet.email
end
On exécute ce fichier avec
$ rails db:seed
si pas d'erreur la main est rendu en console
