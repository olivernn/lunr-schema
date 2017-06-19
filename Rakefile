require 'json-schema'
require 'json'

task :validate => ['schema.json', 'index.json']  do
  JSON::Validator.validate!('schema.json', 'index.json')
end
