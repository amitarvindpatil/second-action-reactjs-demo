## Realistic Exam of Github Action
### Push existing react project on Github repository 
```
1. Download React Project On local Machine
2. cd second-action-react-demo
3. install node js on local
4. npm install
5. npm run dev  --> test on local machine
6. npm test     ---> execute the test case
7. push your code on new repository "second-action-react-demo"
8. git init
9. git add .
10. git commit -m "message"
11. git remote add origin "url"
12. git branch -M main
13. git push -u origin main
```

### Add Workflows for run testcase

```
1. create dir .github/workflows/test.yml

name: Test Project    
on: push     # trigger point of workflow
jobs:        # jobs
  test:      # job name 
    runs-on: ubuntu-latest    # platform OS for run the pipeline
    steps:                     # steps of executions
      - name: Get Code
        uses: actions/checkout@v4     
      - name: Install Nodejs
        uses: actions/setup-node@v4   # uses used to select an action that is already defined and can be reused
        with:               # with declare the configuration
          node-version: '20.x'
      - name: Install Dependancies
        run: npm ci               # use run the commands
      - name: Run Test
        run: npm test 

2. push changes in github -> use Token  

```


### Multiple Jobs and Deployment (parallel/sequentials (just add needs: test)

```
1. create dir .github/workflows/deployment.yml

name: Deploy Project
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get Code
        uses: actions/checkout@v3
      - name: Install Nodejs
        uses: actions/setup-node@v3
        with:
          node-version: '20.x'
      - name: Install Dependancies
        run: npm ci
      - name: Run Test
        run: npm test 
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Get Code
        uses: actions/checkout@v3
      - name: Install Nodejs
        uses: actions/setup-node@v3
        with:
          node-version: '20.x'
      - name: Install Dependancies
        run: npm ci
      - name: Build Project 
        run: npm run build
      - name: Deploy
        run: echo "deploying..."

2. push changes in github -> use Token  

```

#### Events Activity Types and filters
```
- Events such as push,pull request

---------------------------------------
Activity Type
---------------------------------------
pull request Event -> opened,closed,Edited

name: Event Demo
on:  
    # Activity Type Event
    pull_request:
        types:
            - opened
    workflow_dispatch:
jobs:
    deploy:
        runs-on: ubuntu-latest
        steps:
            - name: Output Event Data
              run: echo "${{ toJson(github.event)}}"
            - name: Get code
              uses: actions/checkout@v4
            - name: Install dependencies 
              run: npm ci
            - name: Test code
              run : npm run test
            - name: Build code
              run: npm run build
            - name: deploy project
              run: echo "Deploying......"

2. push changes in github -> use Token       
3. git push - here see, after push the pipeline not trigger but we can mannually trigger the pipeline using workflow_dispatch
4. now create feature branch from main
5. git checkout -b dev
6. make changes in code
7. gut push and create pr and check action
---------------------------------------
Filters
---------------------------------------
Push Event :-> Filter based on target Branch

name: Event Demo
on:  
   
    pull_request:
        types:
            - opened
        branches:
            - main
            - 'dev-*'   # dev-new,dev-this-is-new are valid
            - 'feat/**' # feat/new,feat/new/branch are valid
    workflow_dispatch:

    # Filter Type Event
    push:
        branches:
            - main
            - 'dev-*'   # dev-new,dev-this-is-new are valid
            - 'feat/**' # feat/new,feat/new/branch are valid
        paths-ignore:
            - '.github/workflows/*'
    
jobs:
    deploy:
        runs-on: ubuntu-latest
        steps:
            - name: Output Event Data
              run: echo "${{ toJson(github.event)}}"
            - name: Get code
              uses: actions/checkout@v4
            - name: Install dependencies 
              run: npm ci
            - name: Test code
              run : npm run test
            - name: Build code
              run: npm run build
            - name: deploy project
              run: echo "Deploying.."


name: Event Demo
on:  
    # Activity Type Event
    pull_request:
        types:
            - opened
        branches:
            - main
            - 'dev-*'   # dev-new,dev-this-is-new are valid
            - 'feat/**' # feat/new,feat/new/branch are valid
    workflow_dispatch:
    # Filter Type Event   
    push:
        branches:
            - main
            - 'dev-*'   # dev-new,dev-this-is-new are valid
            - 'feat/**' # feat/new,feat/new/branch are valid
        paths-ignore:
            - '.github/workflows/*'
jobs:
    deploy:
        runs-on: ubuntu-latest
        steps:
            - name: Output Event Data
              run: echo "${{ toJson(github.event)}}"
            - name: Get code
              uses: actions/checkout@v4
            - name: Install dependencies 
              run: npm ci
            - name: Test code
              run : npm run test
            - name: Build code
              run: npm run build
            - name: deploy project
              run: echo "Deploying.."

- push into main branch
```
