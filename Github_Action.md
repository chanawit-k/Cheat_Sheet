# How to GenKey from Docker and Setup Action in GitHub 
- Run Jobs when Code Change
- Automate Tasks
## 1. Generate Key [Url-to-generate-key](https://hub.docker.com/settings/security)

## 2. สร้าง Secret Action ใน GitHub 
    https://github.com/chanawit-k/recipes-app-api/settings/secrets/actions

## 3. Configuring Github Actions
- Create Config file at .github/workflows/checks.yml
```yml
---
name: Checks

on: [push]

jobs:
  test-lint:
    name: Test and Lint
    runs-on: ubuntu-20.04
    steps:
      - name: Login to Docker Hub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USER }} #setup from ข้อ2 
          password: ${{ secrets.DOCKERHUB_TOKEN }} #setup from ข้อ2 
      - name: Checkout #ต้อง checkout code ก่อนเริ่มทำอะไร
        uses: actions/checkout@v2
      - name: Test
        run: docker-compose run --rm app sh -c "python manage.py wait_for_db && python manage.py test"
      - name: Lint
        run: docker-compose run --rm app sh -c "flake8"
```
