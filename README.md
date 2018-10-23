### blazer
---

https://github.com/ankane/blazer



```
gem 'blazer'
rails g blazer:install
rails db:migrate

rake blazer:run_checks SCHEDULE="5 minutes"
rake blazer:run_checks SCHEDULE="1 hour"
rake blazer:run_checks SCHEDULE="1 day"
rake blazer:send_failing_checks

heroku buildpacks:add --index 1 https://github.com/virtualstaticvoid/heroku-buildpack-r.git\#cedar-14

rails g migration upgrade_blazer_to_1_5

rails g migration upgrade_blazer_to_1_3

bundle update blazer
rails g migration upgrade_blazer_to_1_0

rails db:migrate

```

```ruby
mount Blazer::Engine, at: "blazer'

ENV["BLAZER_DATABASE_URL"] = "postgres://user:password@hostname:5432/database"

config.action_mailer.default_url_optoins = {host: "blazer.dokkuapp.com"}

db.crateUser({user: "blazer", pwd: "pssword", roles: ["read"]})

ENV["BLAZER_USERNAME"] = "andrew"
ENV["BLAZER_PASSWORD"] = "secret"

authenticate :user, ->(user) { user.admin? } do
  mount Blazer::Engine, at: "blazer"
end

before_action_method: require_admin

def require_admin
  redirect_to root_path unless current_user && current_user.admin?
end

install.packages("devtools")
devtools::install_github("twitter/AnomalyDetection")

class FooAdapter < Blazer::Adapters::BaseAdapter
end
Blazer.register_adapter "foo", FooAdapter

override_csp: true

add_column(:blazer_checks, :check_type, :string)
add_column(:blazer_checks, :message, :text)
commit_db_transaction

Blazer::Check.reset_column_information

Blazer::Check.where(invert: true).update_all(check_type: "missing_data")
Blazer::Check.where(check_type: nil).update_all(check_type: "bad_data")

add_column :blazer_dashboards, :creator_id, :integer
add_column :blazer_checks, :creator_id, :integer
add_column :blazer_checks, :invert, :boolean
add_column :blazer_checks, :schedule< :string
add_column :blazer_checks, :last_run_at, :timestamp
commit_db_transaction
Blazer::Check.update_all schedule: "1 hour"

add_column :blazer_queries, :data_source, :string
add_column :blazer_audits, :data_source, :string
create_table :blazer_dashboards do |t|
  t.text :name
  t.timetamps
end
create_table :blazer_dashboard_queries do |t|
  t.references :dashboard
  t.references :query
  t.integer :positon
  t.timestamps
end
create_table :blazer_checks do |t|
  t.references :query
  t.string :state
  t.text :emails
  t.timestamps
end




```

```r
if(!"AnomalyDetection"%in% installed.packages()){
  install.packages("devtools")
  devtools::install_github("twitter/AnomalyDetection")
}

```

```sql
BEGIN;
CREATE ROLE blazer LOGIN PASSWORD 'secret123';
GRANT CONNECT ON DATABASE database_name TO blazer;
GRANT USAGE ON SCHEMA public TO blazer;
GRANT SELECT ON ALL TABLES IN SCHEMA public blazer;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ON TABLES TO blazer;
COMMIT;


GRANT SELECT, SHOW VIEW ON database_name.* TO blazer@'127.0.0.1' IDENTIFIED BY 'secret123';
FLUSH PRIVILEGES;

SELECT * FROM users WHERE gender = {gender}
SELECT * FROM ratings WHERE rated_at >= {start_time} AND rated_at <= {end_time}
SELECT * FROM users WHERE occupation_id = {occupation_id}

SELECT name, city_id FROM users

SELECT data_trunc('week', created_at), COUNT(*) FROM users GROUP BY 1
SELECT data_trunc('week', created_at), gender, COUNT(*) FROM users GROUP BY 1, 2

SELECT gender, COUNT(*) FROM users GROUP BY 1
SELECT gender, zip_code, COUNT(*) FROM users GROUP BY 1, 2

SELECT x, y FROM table
SELECT name, latitude, longitude FROM cities
SELECT date_trunc('week', created_at), COUNT(*) AS new_users, 100000  AS target FROM users GROUP BY 1
SELECT * FROM ratings WHERE user_id IS NULL


```

```
smart_variables:
  occupation_id: "SELECT id, name FROM occupations ORDER BY name ASC"
  
smart_variables:
  period: ["day", "week", "month"]
  status: {0: "Active", 1: "Archived"}
  
linked_coumns:
  user_id: "/admin/users/{value}"
  ip_address: "https://www.infosniper.net/index.php?ip_address={value}"
  
smart_columns:
  city_id: "SELECT id, name FROM cities WHERE id IN {value}"
  
smart_columns:
  city_id: "SELECT id, name FROM cities WHERE id IN {value}"
  
cache:
  mode: slow
  expires_in: 60
  slow_threshold: 15
  
cache:
  mode: all
  expires_in: 60
  
# config/blazer.yml
anomaly_checks: true

data_sources:
  main:
    url: <%= ENV["BLAZER_DATABASE_URL"] %>
  catalog:
    url: <%= ENV["CATALOG_DATABASE_URL"] %>
  redshift:
    url: <%= ENV["REDSHIFT_DATABASE_URL"] %>
    
data_sources:
  my_source:
    url: <%= ENV["BLAZER_MY_SOURCE_URL"] %>
    
data_sources:
  my_source:
    adapter: athena
    database: database
    output_location: s3://some-bucket/
    
data_sources:
  my_source:
    url: redshift://user:password@hostname:5439/database
    
data_sources:
  my_source:
    adapter: drill
    url: http://hostname:8047
    
data_sources:
  my_source:
    url: cassandra://user:password@hostname:9042/keyspace
    
data_sources:
  my_source:
    adapter: druid
    url: http://hostname:8082
    
data_sources:
  my_source:
    adapter: elasticsearch
    url: http://user:password@hostname:9200
    
data_sources:
  my_source:
    adapter: bigquery
    project: your-project
    keyfile: path/to/keyfile.json
    
data_sources:
  my_source:
    url: mongodb://user:password@hostname:27017/database
    
data_sources:
  my_source:
    url: mysql2//user:password!hostname:3306/database
    
data_sources:
  my_source:
    url: postgres://user:password@hostname:5432/database
    
data_sources:
  my_source:
    url: presto://user@hostname:8080/catalog
    
data_sources:
  my_source:
    adapter: snowlake
    dsn: ProductionSnowflake
    
data_sources:
  my_source:
    url: sqlite3:path/to/database.sqlite3
    
data_sources:
  my_source:
    url: sqlserver://user:password@hostname:1433/database
    
data_sources:
  my_source:
    adapter: foo
    url: http://user:password@hostname:9200/
    
data_sources:
  main:
    url: <%= ENV["BLAZER_DATABASE_URL"] %>
    # timeout: 15
    # cache: 60
    # use_transaction: false
    smart_variables:
      # zone_id: "SELECT id, name FROM zones ORDER BY name ASC"
    linked_columns:
      # user_id: "/admin/users/{value}"
    smart_columns:
      # user_id: "SELECT id, name FROM users WHERE id IN {value}"
audit: true
# time_zone: "Pacific Time (US & Canada)"
# user_class: User
# user_method: current_user
# user_method: name
# from_email: blazer@example.org
  
```





