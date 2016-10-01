# Ruby/Rails, gem/bundler

### Resolve openssl build issue on Mac OS X 10.11.*

`bundle config build.eventmachine --with-cppflags=-I/usr/local/opt/openssl/include`

### Rollback selected migration

`rails db:migrate:down VERSION=20160920224225`
