### CHECK THE COMMITS BETWEEN `tag` AND `master` WITH DATE AND AUTHOR. 
```bash
git log --pretty=format:"%h%x09%an%x09%ad%x09%s" tag..master
```