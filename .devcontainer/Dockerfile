# Create a GitHub Pages container with the required Jekyll dependencies from a Debian Bullseye image with Ruby

# Currently GitHub requires Ruby 2.7.x, see https://pages.github.com/versions/
FROM ruby:2.7-bullseye

# Update image
RUN apt update && apt upgrade -y

# Update dependency manager and install GitHub Pages and its dependencies
RUN gem update bundler && gem install github-pages

WORKDIR /var/www/

EXPOSE 4000

CMD [ "jekyll", "serve", "--livereload", "--force_polling", "-H", "0.0.0.0", "-P", "4000" ]

# usage without development containers:
# docker run -it --rm -v "$PWD":/var/www -p "4000:4000" <image_name>
