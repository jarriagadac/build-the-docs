# makefile

build:
	docker-compose --file dc-produccion.yml --project-name build_the_docs up -d --build --force-recreate --remove-orphans
	docker ps

_build:
	docker-compose --project-name django_build_the_docs up -d --build --force-recreate --remove-orphans
	make _migrate
	docker exec django_build_the_docs make precommit
	docker ps

_admin:
	docker exec django_build_the_docs bash -c "echo \"from django.contrib.auth.models import User; User.objects.create_superuser('admin', '', 'admin')\" | python manage.py shell"

restart:
	docker restart django_build_the_docs
	docker ps

update:
	git submodule update
	git stash
	git pull
	git stash pop

_static:
	docker exec django_build_the_docs python manage.py collectstatic --no-input

media:
	sudo cp -r ../media/* /home/instalar/media/django_build_the_docs

_migrate:
	docker exec django_build_the_docs python manage.py migrate

loaddata:
	docker exec django_build_the_docs make loaddata

logs:
	docker logs -f django_build_the_docs

ssh:
	docker exec -it django_build_the_docs bash

deploy:
	make update
	make build
	make migrate
	# make loaddata
	make static
	make media
	make restart

_secret:
	./app/secret.sh
