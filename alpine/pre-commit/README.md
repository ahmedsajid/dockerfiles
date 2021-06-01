To run the container with git repo and pre-commit-config.yaml in current directory
```docker run --name pre-commit-test-container -v $(pwd):/tmp/repo -w /tmp/repo ahmedsajid/pre-commit run --all-files```
