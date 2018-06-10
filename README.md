# ui_tests_framework
To build the image:
docker build -f RobotFrameworkHub/Dockerfile -t localrepo/robothub

docker-compose -f docker-compose.yml up -d
will setup 3 containers:
	- selenium-hub: selenium grid + robot framework + xvfb
	- chrome_node: a container with headless chrome webdriver running
	- firefox_node: a container with headless firefox webdriver running


