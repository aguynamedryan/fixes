

## Solution

I fiddled with passenger.{conf,load} and reinstalled passenger from packages provided by the OS.  No joy.

I added `Options -Indexes` to the `<Directory "/var/www/wedn/public">` entry, but that just changed the problem so that I get a `403 - Forbidden` error, which I guess is slightly better ;-)

I updated the Directory section to use the newer syntax for Apache2:

```Apache
AllowOverride all
Require all granted
Options -MultiViews
```

That didn't really help though, but it is probably important.

So then I realized that I was missing the config.ru file, so I found the version in Rails guide here http://guides.rubyonrails.org/v2.3.11/rails_on_rack.html#rackup and made a config.ru file that reads like:

```ruby
use Rack::Lock
use ActionController::Failsafe
use ActionController::Session::CookieStore, , {:secret=>"<secret>", :session_key=>"_<app>_session"}
use Rails::Rack::Metal
use ActionController::RewindableInput
use ActionController::ParamsParser
use Rack::MethodOverride
use Rack::Head
use ActiveRecord::QueryCache
run ActionController::Dispatcher.new
```

But the ActionController::Session::CookieStore line is junk, so don't use that ;-)

After that I got a passenger screen, but it showed an error of "uninitialized constant ActionController"

So, I used a smaller set of lines and also added a require statement, so now the file reads:

```ruby
require File.expand_path(File.dirname(__FILE__) + '/config/environment')
use Rails::Rack::LogTailer
use Rails::Rack::Static
run ActionController::Dispatcher.new
```

And that worked!
