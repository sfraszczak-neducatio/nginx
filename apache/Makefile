project=apache-oeo
http_port=9001

install:
	@docker build -t apache/oeo .

run:
	@echo "\033[1;32mApplication info:"
	@echo -n "\033[0;32m   - ID:\t\t\033[1;32m"
	@docker run -p $(http_port):80 -v $(CURDIR)/app:/var/www:rw --name $(project) -d apache/oeo /sbin/my_init --enable-insecure-key
	@echo -n "\033[0;32m   - Tag:\t\t\033[1;32m"$(project)"\n"
	@echo -n "\033[0;32m   - IP address:\t\033[1;32m"
	@docker inspect -f "{{ .NetworkSettings.IPAddress }}" $(project)
	@echo "\033[0;32m   - Localhost:\t\t\033[1;32mlocalhost:"$(http_port)"\033[0m"
	@echo -n "\033[0;32m   - Running:\t\t\033[1;32m"
	@docker inspect -f "{{ .State.Running }}" $(project)
	@echo "\033[0m"

status:
	@echo -n "\033[32mApplication is available on:\n\033[0m   - "
	@docker inspect -f "{{ .NetworkSettings.IPAddress }}" $(project)
	@echo "   - localhost:"$(http_port)

.PHONY: install run status