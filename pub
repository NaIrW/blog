from db import DB
from sys import argv
from hashlib import sha256
from datetime import date


db = DB('nairw.blog')


def getById(blog_id):

    if blog_id in db:
        return db.find(blog_id)
    else:
        return None
    

def pubNewBolg(file_path):
    hash = None
    with open(file_path, 'r', encoding='utf-8') as f:
        head = f.readline().strip()
        if '=' in head:
            hash = head.split('=')[1]
        title = eval(f.readline().strip())
        name = eval(f.readline().strip())
        tag = eval(f.readline().strip())
        f.readline()
        content = f.read()

    db = DB('nairw.blog')
    print(hash)
    if hash:
        db.update(hash, {
        'hidden': True,
        'title': title,
        'name': name,
        'date': date.today().strftime('%Y/%m/%d'),
        'tags': tag.split(','),
        'content': content,
        'sign': 'BY_DIRECT_EDID_ON_DATABASE'
    })
    else:
        db.insert(sha256((title + name + tag + content).encode()).hexdigest(), {
            'hidden': True,
            'title': title,
            'name': name,
            'date': date.today().strftime('%Y/%m/%d'),
            'tags': tag.split(','),
            'content': content,
            'sign': 'BY_DIRECT_EDID_ON_DATABASE'
        })
        with open(file_path, 'r', encoding='utf-8') as f:  
            data = f.read()
        with open(file_path, 'w', encoding='utf-8') as f:  
            f.write(f'```python={sha256((title + name + tag + content).encode()).hexdigest()}\n' + data.split('\n', 1)[1])



def pubBolg(file_path, blog_id):
    with open(file_path, 'r', encoding='utf-8') as f:
        f.readline()
        title = eval(f.readline().strip())
        name = eval(f.readline().strip())
        tag = eval(f.readline().strip())
        f.readline()
        content = f.read()

    db = DB('nairw.blog')
    db.update(blog_id, {
        'hidden': True,
        'title': title,
        'name': name,
        'date': date.today().strftime('%Y/%m/%d'),
        'tags': tag.split(','),
        'content': content,
        'sign': 'BY_DIRECT_EDID_ON_DATABASE'
    })


if __name__ == '__main__':
    if len(argv) == 2:
        pubNewBolg(argv[1])
    elif len(argv) == 3:
        pubBolg(argv[1], argv[2])
    else:
        print('wrong usage')
