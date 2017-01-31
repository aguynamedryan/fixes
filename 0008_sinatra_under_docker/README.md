## Solution

In the past, I've used the awesome [Sinatra Docker Container](https://github.com/erikap/docker-ruby-sinatra) to drive some of my sinatra-based apps.  However, I wanted to test out the ability to create a Docker image that completely contains a Sinatra-based application that I can just fire up and have it start serving up the app immediately.

My problem was that when I'd fire up the sinatra app, nothing could talk to it.  I played around with Docker run's [-P and -p flags](https://docs.docker.com/engine/reference/run/#expose-incoming-ports) to no avail.  I was thinking that it was a problem with running Docker under OS X, but I still had the problem when running on Linux directly.

Finally, I read on this [cool blog post](https://rubyplus.com/articles/2461-Docker-Basics-Running-a-Hello-World-Sinatra-App-in-a-Container) that I needed to run `set :bind, "0.0.0.0"` at the top of the Sinatra app.rb file.

If you'd like to see the final product, [this commit of Explodacode](https://github.com/outcomesinsights/explodeacode/commit/57dfa29a0eb65ea9dc06fe77d5a0d371f41bf911) has Dockerfile and changes to app.rb.
