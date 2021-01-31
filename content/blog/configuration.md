---
title: "Configuration"
date: 2021-01-31T20:56:18+09:00
draft: true

#post thumb
image: "images/post/configuration/configuration.png"

# meta description
description: "this is meta description"
math: true

# taxonomies
categories:
  - "Python"
tags:
  - "Usage"

# post type
type: "post"
---

Python 을 이용하여 ML, DL 코딩을 하다보면 관리해야하는 설정값이 매우 많습니다.  
너무 많아서 문제입니다... `Backbone, Learning rate, Epochs, Save path, ......`
이걸 실행할때 마다 스크립트를 수정하기에는 매우 번거롭습니다.  
그렇기에 이런 설정값을 관리하는 Configuration 파일을 따로 만들어서 관리를 하죠!  
이번 포스팅에선 대표적으로 사용되는 네 가지 방법을 포스팅하려 합니다.
각각에 대한 구체적인 설명을 하지 않습니다.  
예시 코드만 적고 포스팅을 마치려 합니다. 

### Json
``` json
{
"name": "Jerry",
"github": "jjerry-k",
"blog_url": "jjerry-k.github.io",
"sns": {
  "facebook": "https://www.facebook.com/jerry.kim.566/",
  "twitter": "https://twitter.com/jjerry_k",
  "linkdin": "https://www.linkedin.com/in/jerry-kim-b8804216b/"
},
"favorite": ["cat", "coding", "DIY"]
}
```

```python
import json

with open("config.json") as json_data_file:
    data = json.load(json_data_file)

print(type(data)) # <class 'dict'>
print(data)
# {'name': 'Jerry', 
# 'github': 'jjerry-k', 
# 'blog_url': 'jjerry-k.github.io', 
# 'sns': {'facebook': 'https://www.facebook.com/jerry.kim.566/', 
#         'twitter': 'https://twitter.com/jjerry_k', 
#         'linkedin': 'https://www.linkedin.com/in/jerry-kim-b8804216b/'}, 
# 'favorite': ['cat', 'coding', 'DIY']}
```

### YML/YAML
``` yaml
name: Jerry

github: jjerry-k

sns:
    facebook: https://www.facebook.com/jerry.kim.566/
    twitter: https://twitter.com/jjerry_k
    linkedin: https://www.linkedin.com/in/jerry-kim-b8804216b/

favorite: 
    - cat
    - coding
    - DIY
```

```python
import yaml

with open("config.yml", "r") as ymlfile:
    data = yaml.load(ymlfile)

print(type(data)) # <class 'dict'>
print(data)
# {'name': 'Jerry', 
# 'github': 'jjerry-k', 
# 'blog_url': 'jjerry-k.github.io', 
# 'sns': {'facebook': 'https://www.facebook.com/jerry.kim.566/', 
#         'twitter': 'https://twitter.com/jjerry_k', 
#         'linkedin': 'https://www.linkedin.com/in/jerry-kim-b8804216b/'}, 
# 'favorite': ['cat', 'coding', 'DIY']}
```

### XML
``` xml
<config>
    <name>Jerry</name>
    <github>jjerry-k</github>
    <blog_url>jjerry-k.github.io</blog_url>
    <sns>
        <facebook>https://www.facebook.com/jerry.kim.566/</facebook>
        <twitter>https://twitter.com/jjerry_k</twitter>
        <linkedin>https://www.linkedin.com/in/jerry-kim-b8804216b/</linkedin>
    <favorite>
        <li>cat</li>
        <li>coding</li>
        <li>DIY</li>
    </favorite>
</config>
```

``` python
import xml.etree.ElementTree as ET
root = ET.parse('config.xml').getroot()

print(type(root)) # <class 'xml.etree.ElementTree.Element'>
elem_list = list(root)
for elem in elem_list:
    sub_elem_list = list(elem)
    if sub_elem_list:
        for sub_elem in sub_elem_list:
            print(f"{elem.tag}, {sub_elem.tag}: {sub_elem.text}")
    else: print(f"{elem.tag}: {elem.text}")

# name: Jerry
# github: jjerry-k
# blog_url: jjerry-k.github.io
# sns, facebook: https://www.facebook.com/jerry.kim.566/
# sns, twitter: https://twitter.com/jjerry_k
# sns, linkedin: https://www.linkedin.com/in/jerry-kim-b8804216b/
# favorite, li: cat
# favorite, li: coding
# favorite, li: DIY
```

### cfg/conf
``` cfg
[defaults]
name= Jerry
github= jjeryr-k
blog_url= jjerry-k.github.io
favorite= ["cat", "coding", "DIY"]

[sns]
facebook= https://www.facebook.com/jerry.kim.566/
twitter= https://twitter.com/jjerry_k
linkedin= https://www.linkedin.com/in/jerry-kim-b8804216b/
```

``` python
import configparser

config = configparser.ConfigParser()
config.read('config.cfg')

print(type(config)) # <class 'configparser.ConfigParser'>

for section in config.sections():
    for option in config.options(section):
        print(f"{section}, {option}, {config.get(section, option)}")

# defaults, name, Jerry
# defaults, github, jjeryr-k
# defaults, blog_url, jjerry-k.github.io
# defaults, favorite, ["cat", "coding", "DIY"]
# sns, facebook, https://www.facebook.com/jerry.kim.566/
# sns, twitter, https://twitter.com/jjerry_k
# sns, linkedin, https://www.linkedin.com/in/jerry-kim-b8804216b/

```

여기까지 입니다.  
흠....개인적으론 YAML이 가장 편해보이네요.  
configuration file 만드는거나...parsing 하는거나...  
본인 편한거 잘 찾아서 쓰시면 될 듯합니다!

### P.S
- 다음엔 뭐쓰지...