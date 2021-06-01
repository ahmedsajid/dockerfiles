To run the pre-commit check with 'run --all-files' option, with git repo and pre-commit-config.yaml in pwd
```docker run --name pre-commit-test-container -v $(pwd):/tmp/repo -w /tmp/repo ahmedsajid/pre-commit```
