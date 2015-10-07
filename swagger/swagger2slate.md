Always update swagger2slate.phar from [Github](https://github.com/E96/swagger2slate).
Then, `chmod +x swagger2slate.phar`.

This solution works only with swagger.json file, therefore make to export the swagger.yaml in JSON from the editor. Then:
    
    cd swagger
    ./swagger2slate.phar convert ../../solution/modules/api/src/main/swagger/swagger.json -o ../source/index.md
