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
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get Code
        uses: actions/checkout@v4
      - name: Install Nodejs
        uses: actions/setup-node@v4
        with:
          node-version: '20.x'
      - name: Install Dependancies
        run: npm ci
      - name: Run Test
        run: npm test 

2. push changes in github -> use Token

```