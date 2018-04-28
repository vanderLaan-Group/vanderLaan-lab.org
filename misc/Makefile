.PHONY : all
all: build deploy

build:
	Rscript -e "blogdown::build_site()"

serve:
	Rscript -e "blogdown::serve_site()"

deploy:
	sh deploy.sh

# default post title
# TITLE="title"
# to create a new post with a particular title:
# make new TITLE="blah blah blah"
new:
	Rscript -e "blogdown::new_post(title='\'$(TITLE)\'', author='Mark', rmd=T)"
	vim content/post/
