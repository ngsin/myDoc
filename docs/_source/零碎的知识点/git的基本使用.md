## git基本使用

初始化

```bash
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/KevinJohn-GH/test.git
git push -u origin main
```