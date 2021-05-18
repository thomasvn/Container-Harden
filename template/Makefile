help: ## Show this help
	@egrep -h '\s##\s' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'

build: ## Build the template container image
	- docker rmi template
	docker build . -t template

debug: ## Build and run a shell in the template container
	- docker stop tmp
	make build
	# docker run -it --rm -p 8080:80 --name tmp template bash
	docker run -d --rm -p 8080:80 --name tmp template
	docker exec -it tmp bash
