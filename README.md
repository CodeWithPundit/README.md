curl -s "https://api.github.com/users/YOUR_USERNAME/repos?per_page=100" | python3 -c "
import json,sys,subprocess,urllib.request,math,datetime
repos=json.load(sys.stdin)
stars=sum(r['stargazers_count'] for r in repos)
forks=sum(r['forks_count'] for r in repos)
langs={}
for r in repos:
    if r['language']:
        langs[r['language']]=langs.get(r['language'],0)+1
top_lang=sorted(langs.items(),key=lambda x:x[1],reverse=True)[0][0] if langs else 'None'
user_data=json.loads(urllib.request.urlopen('https://api.github.com/users/YOUR_USERNAME').read())
today=datetime.datetime.now().strftime('%Y-%m-%d')
with open('README.md','w') as f:
    f.write(f'''# 🚀 {user_data[\"name\"] or user_data[\"login\"]}
    
## 📊 GitHub Analytics
**Repositories:** {user_data['public_repos']} • **Stars:** {stars} • **Forks:** {forks}
**Top Language:** {top_lang}
**Most Starred Repo:** {max(repos,key=lambda x:x['stargazers_count'])['name']} ({max(repos,key=lambda x:x['stargazers_count'])['stargazers_count']} ⭐)

## 🛠️ Tech Stack (Based on Repository Analysis)
{chr(10).join(f'- {l}: {c} repos' for l,c in sorted(langs.items(),key=lambda x:x[1],reverse=True)[:5])}

## 🔥 Recent Activity
- Check my contribution graph: [github.com/YOUR_USERNAME](https://github.com/YOUR_USERNAME)
- Last updated: {today}

## 📬 Connect
📧 Add your email here
💼 Add your LinkedIn here
🐦 Add your Twitter here

---
*Auto-generated profile - Update as needed!*''')
print('README.md created!')
"
