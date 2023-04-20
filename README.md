<div align="center">
  ![Ruby](https://img.shields.io/badge/ruby-%23CC342D.svg?style=for-the-badge&logo=ruby&logoColor=white)
  
  ![Rails](https://img.shields.io/badge/rails-%23CC0000.svg?style=for-the-badge&logo=ruby-on-rails&logoColor=white) 
  ![MongoDB](https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white) 
  ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) 
  ![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
</div>

<br/>
<p align="center">
  <a href="https://www.coverwallet.com/">
    <img src="images/logo.png" alt="Logo" width="400" height="100">
  </a>

<h3 align="center">Epic Adapter</h3>
</p>

<p align="center">
  This service is designed for <a href="https://coverwallet.atlassian.net/l/cp/e2hWRJ7p"> accounting flows Synchronizations</a> between CoverWallet and US accounting team using EPIC
</p>

## Table of Contents

- [Installation](#installation)
- [Developing](#developing)
- [Tests](#tests)
- [Deployment](#deployment)

## Installation

  1. Clone Github repo and install Ruby gems

      ```bash
      git@github.com:coverwallet/cw-rails-epic-adapter.git
      gem install bundler
      bundle install
      ```

  2. Get development credentials:

      ```bash
      sops -d --input-type=dotenv dev.env > .env
      ```

  3. Requirements

      You should have already installed and **running**:

      - Redis
      - Mongodb
      - Ruby

      You can check it by running the following command

      ```bash
      brew services list
      ```

## 2. Developing

  1. Run the app

      To run the app run the following command

      ```bash
      rails server
      ```

      Or could be used docker to run the app. If you don't have it you will need to install it in your machine. You can see how [(here)](https://docs.docker.com/get-docker/)

      ```bash
      docker-compose up web-dev
      ```

      When the app is running may you need to access the admin panel on your local machine, to do this you need to access the rails console with the following command

      ```bash
      rails console
      ```

      Once you are in the rails console, run the following commands with your data to register a new Admin User (minimum length of characters for the password is 6)

      ```ruby
      new_admin_user = AdminUser.new(email: "example@coverwallet.com", password:'test01', password_confirmation:'test01')
      new_admin_user.save
      ```

      Now you are able to [login](http://localhost:3000/admin)

  2. Adding a new application mode 

      To add a new application mode we need to add the Application Mode Name in the ```DEPARTMENT_CODES``` constant in the ```BusinessStructure``` class at ```app/services/epic/business_structure.rb``` and add the respective Factory for Rspec test suites.
      To allow the application mode to be processed correctly you need to create a PublicApiConfig, there are two ways to do this, we can add it from the UI in the admin panel or we can add it directly in the rails console with the following command

      ```ruby
      new_public_api_config = PublicApiConfig.new(name: "TEST", url: "https://www.test.biz/en-us/test/public-api", client_id: "a4b0ac0d-d462-4902-b2cb-1c67474ae819", client_secret: "ew1HIAyatky_yAhDqdy5u1XfoDM5zNfoRNGes5_1A3E")
      new_public_api_config.save
      ```

## Tests

  Tests could be run with:

  ```bash
  rspec
  ```

## Deployment

  Thanks to CI, it's really easy to deploy the project to production.

  In the [#deploys](https://coverwallet.slack.com/archives/C33LQPZ7T) slack channel: 


  - If you want to view your changes: `@candi changes cw-rails-epic-adapter`
  - For deployment, you just need to write: `@candi deploy cw-rails-epic-adapter to production`
