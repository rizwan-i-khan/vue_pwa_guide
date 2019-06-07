<h2>Guide for installing vuestorefront pwa. </h2>
Vue Storefront is the biggest and most complete PWA for magento sites.

<h4>Some of the key elements of vuestorefront.</h4>
1: All devices and platforms
	<p>Deliver a 100% responsive and smooth shopping experience without developing apps for different web browsers and platforms like iOS, Android, windows Etc.</p>

2: Incredible performance
	<p>With help of node.js, vue.js and excellent search mechanism of Elasticsearch give incredible performance. vuestorefront can achieve Google page speed Accessibility score 89, Performance score 89, SEO score 92 and 100 PWA score.</p>

3: Native functionalities.
	<p>Take advantage of push notifications, home-screen install access[currently for android], and full-screen mode. It also make sure that store works in offline mode with poor connection. and it also support functionality to place orders offline!!</p>


<h3>VueStorefront Prerequisites and system requirements.</h3>
For setting up vuestorefront environment you must have to install below components.

1: Latest version of Docker. <br/>
	https://medium.com/@Grigorkh/how-to-install-docker-on-ubuntu-16-04-3f509070d29c  
	
2: Node.js, version > 8.0.0, and yarn. <br/>
	First install node.js and npm. <br/>
		https://itsfoss.com/install-nodejs-ubuntu/ <br/>
	Install yarn. <br/>
		https://www.hostinger.in/tutorials/how-to-install-yarn-on-ubuntu/ <br/>
		
3: Install imagemagick library. <br/>
	https://tecadmin.net/install-imagemagick-on-linux/ <br/>

4: Elasticsearch and Redis server. <br/>
	Redis: https://tecadmin.net/install-redis-ubuntu/ <br/>
	elastic search: https://tecadmin.net/setup-elasticsearch-on-ubuntu/  <br/>


There are two methods to setup vuestorefront. <br/>
1: Using CLI installation. <br/>
	You can install vuestorefront anywhere on server but we preferred it to be resides in magento directory. <br/>

	1: First install vuestorefront using below command.
		`git clone https://github.com/DivanteLtd/vue-storefront.git vue-storefront`

	2: Download all npm packages by executing below command in vue-storefront directory.
		`yarn install`

	3: run installer.
		npm run installer

	4: In first step you need to provide you magento url.

	5: Provide you git variable in most of the cases it will be `git`

	6: Path for installing backend locally.
		enter `vue-storefront-api` it will clone api repository in `vue-storefront-api` directory.

	7: Path for images endpoint.
		http://localhost:8080/img/

	8: It takes some time for installation and after successful installation you can see success screen.

	if you see success screen that means.
		- You got Redis and Elastic Search up and running.
		- vue-storefront is up and running on localhost:3000
		- vue-storefront-api us running on localhost:8080

	=> Working with local instances.
		Our basic instance are ready and we can move to integrate with local magento instance.
		First you need to install mage2vuestorefront — which is simple. 
		https://github.com/DivanteLtd/mage2vuestorefront

		1: git clone https://github.com/DivanteLtd/mage2vuestorefront.git mage2vs cd mage2vs/src npm install

		2: We need to create a integration for api calling of magento2 default.
			In magento backend go to System -> Integrations then Then click Add new integration and just fill:
			- Name (whatever)
			- your password to confirm the changes,
			- check Catalog, Sales, My Account and Carts on API permissions tab — save

		3: In the result you’ll click Activate and get some oauth access tokens:

		4: Please edit the src/config.js file in your mage2vuestorefront directory and provide magento_url, consumerKey, consumerSecret, accessToken, accessTokenSecret key.
			- The rest of config.js files points out to your vue-storefront-api based Docker and Redis instances which are required by mage2nosql to work.
			- To import all the Products, Categories and other important stuff to your Elastic Search instance you should run the following commands
				node cli.js taxrule
				node cli.js attributes
				node cli.js categories
				node cli.js productcategories
				node cli.js products
			- It’s advisable to run these commands over and over as they’re doing update operation in elastic search.

	=>How to restart everything?
		goto vue-storefront directory and run npm run dev
		goto vue-storefront-api directory and run: docker-compose up -d and then npm run dev

2: Manual installation,
	If above fails for you, you can also do manual steps.

	1: clone vuestorefront repo to your magento root folder.
	- git clone https://github.com/DivanteLtd/vue-storefront.git vue-storefront
	- install dependencies using yarn install

	2: clone vuestorefrontapi
		- git clone https://github.com/DivanteLtd/vue-storefront-api.git vue-storefront-api
		- install dependencies using yarn install or npm i

	3: copy default config to local config in vue-storefront/config/ and vue-storefront-api/config.

	4: create integration for magento2 api call and provide details [token info] in vue-storefront-api/config/local.json

	5: go to vuestorefront-api folder and up the docker.
		docker-compose up -d

	6: import data of magento2 to elasticsearch.
		-> yarn mage2vs import
		-> varify your data on http://localhost:5601/

	7: run below command form vuestorefront and vuestorefront-api
		-> npm run dev

	8: open localhost:3000 and varify your site.

	Restart all instances in manual mode.
	1: go to vuestorefront-api folder and up the docker.
	docker-compose up -d

	2: import data of magento2 to elasticsearch.
		-> yarn mage2vs import
		-> varify your data on http://localhost:5601/
		-> indices: vue_storefront_catalog

	3: run below command form vuestorefront and vuestorefront-api
		-> npm run dev

	4: open localhost:3000

=> Additional tips and commands. <br/>
	- Upgrade your npm to latest version <br/>
		npm install npm -g <br/>
	- Stop redis: sudo systemctl stop redis-server.service <br/>
	- Stop elastic search: sudo systemctl stop elasticsearch.service <br/>
	- After importing data to elastic search you can check your data by hitting KIBANA. <br/>
		http://localhost:5601/

