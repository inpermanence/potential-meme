import requests,re
url = 'https://findcumt.libsp.com/find/unify/search'
detailurl = 'https://findcumt.libsp.com/find/physical/groupitems'
result = requests.post(url)
headers = {
    
'Host': 'findcumt.libsp.com',
'Connection': 'keep-alive',
'Content-Length': '520',
'sec-ch-ua': '" Not A;Brand";v="99", "Chromium";v="96", "Microsoft Edge";v="96"',
'groupCode': '200069',
'sec-ch-ua-mobile':'?0',
'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.55 Safari/537.36 Edg/96.0.1054.41',
'Content-Type': 'application/json;charset=UTF-8',
'Accept': 'application/json, text/plain, */*',
'mappingPath':"",
'x-lang': 'CHI',
'sec-ch-ua-platform': "Windows",
'Origin':'https://findcumt.libsp.com',
'Sec-Fetch-Site': 'same-origin',
'Sec-Fetch-Mode': 'cors',
'Sec-Fetch-Dest': 'empty',
'Referer': 'https://findcumt.libsp.com/',
'Accept-Encoding': 'gzip, deflate, br',
'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6',
'Cookie': 'SameSite=None'
} 
a = str(input())                                                        #输入搜索内容
params = {"docCode":None,"searchFieldContent":a,"searchField":"keyWord","matchMode":"2","resourceType":[],"subject":[],"discode1":[],"publisher":[],"libCode":[],"locationId":[],"eCollectionIds":[],"curLocationId":[],"campusId":[],"kindNo":[],"collectionName":[],"author":[],"langCode":[],"countryCode":[],"publishBegin":"null","publishEnd":"null","coreInclude":[],"ddType":[],"verifyStatus":[],"group":[],"sortField":"relevance","sortClause":"asc","page":1,"rows":10,"onlyOnShelf":"null","searchItems":"null","keyWord":[]}
result =requests.post(url,json=params,headers=headers)                  #爬取检索页面
p=str(result.content,encoding="utf-8")                                  #解析检索页面数据

name = re.findall("title",p)

list = []

for i in range (0,10):
    id = re.findall(r'"recordId":([0-9]+),',p) [i]                                           #提取数据recordId
   
 
    data = {"page":1,"rows":"20","entrance":None,"recordId":id,"isUnify":"true"}                #详细信息页面参数
    response = requests.post(detailurl,json=data,headers=headers)                               #爬取详细页面

static = response.json()
result1 = result.json()
for i in range (0,10):
    information =[]
    information.append("name: ")
    information.append(result1['data']['searchResult'][i]['title'])
    information.append("bookid: ")
    information.append(result1['data']['searchResult'][i]['recordId'])
    information.append("publisher: ")
    information.append(result1['data']['searchResult'][i]['publisher'])
    information.append("isbn: ")
    information.append(result1['data']['searchResult'][i]['isbn'])
    information.append("physicalCount: ")
    information.append(result1['data']['searchResult'][i]['physicalCount'])
    information.append("locationname: ")
    information.append(static['data']['list'][i]['locationName'])
    information.append('processType: ')
    information.append(static['data']['list'][i]['processType'])
    information.append("barcode: ")
    information.append(static['data']['list'][i]['barcode'])
    information.append("callNo: ")
    information.append(static['data']['list'][i]['callNo'])
      

    print (information)
