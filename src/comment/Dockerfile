FROM ruby:2.2
RUN apt-get update -qq && apt-get install -y build-essential

ENV APP_HOME /app
ENV COMMENT_DATABASE_HOST comment_db
ENV COMMENT_DATABASE comments

WORKDIR $APP_HOME

COPY Gemfile* $APP_HOME/
RUN bundle install
COPY . $APP_HOME

CMD ["puma"]
