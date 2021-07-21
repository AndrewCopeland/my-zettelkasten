# 1626883474 node-typescript-project-setup
Make a new directory where you project will live:
```bash
mkdir project_name
cd project_name
```

Initialize this folder as a npm project:
```bash
npm init -y
```

Since we are using typescript with this project we will need to install the typescript dependancies:
```bash
npm install --save-dev typescript@3.3.3
npm install --save-dev tslint@5.12.1
```

Once typescript dependancies have been installed, you must initialize the typescript project by executing the following command:
```bash
tsc --init
```

This command will create the `tsconfig.json` file which contains configuration parameters for the typescript project.


Next step is to configure TypeScript linting by executing the following command:
```bash
./node_modules/.bin/tslint --init
```

A new file `tslint.json` should be created.


## Links
