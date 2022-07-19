
# Create a GitHub Pages container with the required Jekyll dependencies from a Ruby Alpine image

# Currently GitHub requires Ruby 2.7.x, see https://pages.github.com/versions/
FROM ruby:2.7-alpine

# Add Jekyll dependencies to Alpine Ruby image
RUN apk --no-cache add build-base gcc

# Update dependency manager and install GitHub Pages and its dependencies
RUN gem update bundler && gem install github-pages

WORKDIR /var/www/

EXPOSE 4000

CMD [ "jekyll", "serve", "--livereload", "--force_polling", "-H", "0.0.0.0", "-P", "4000" ]

# usage without Visual Studio Code:
# docker run -it --rm -v "$PWD":/var/www -p "4000:4000" <image_name>
