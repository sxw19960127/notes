# 全国城市名字排序

## 需求：将城市按照首字母进行排序&&检索出热门城市

```js
var cities = {
    "cityList": [
        {
            "id": 1,
            "nm": "北京",
            "isHot": 1,
            "py": 'beijing'
        },
        {
            "id": 10,
            "nm": '上海',
            "isHot": 1,
            "py": 'shanghai'
        },
        {
            "id": 20,
            "nm": '广州',
            "isHot": 1,
            "py": 'guangzhou'
        },
        {
            "id": 30,
            "nm": '杭州',
            "isHot": 0,
            "py": 'hangzhou'
        },
        {
            "id": 42,
            "nm": '兰溪',
            "isHot": 0,
            "py": 'lanxi'
        },
        {
            "id": 39,
            "nm": '安徽',
            "isHot": 0,
            "py": 'anhui'
        },
        {
            "id": 31,
            "nm": '长春',
            "isHot": 0,
            "py": 'chuangchun'
        },
        {
            "id": 32,
            "nm": '上饶',
            "isHot": 0,
            "py": 'shangrao'
        }
    ]
}
var cities = cities.cityList

function formatCityList(cities) {
    // 最终排序完成的数据
    let cityListResult = []
    // 最终筛选出的热门城市
    let hotList = []
    
    // 筛选出热门城市,即isHot字段为1的元素
    for(var i = 0;i < cities.length;i ++) {
		if( cities[i].isHot === 1 ) {
            hotList.push( cities[i] )
        }
    }
    
    for(let i = 0;i < cities.length;i ++) { // 循环遍历数组中的每一项
        // [{},{},{},{}...]
        // 按首字母顺序排序数组中的元素
        // 使用substring(0,1)截取每一项的首字母并转为大写形式
        let firstLetter = cities[i].py.substring(0,1).toUpperCase()
        
        if( judge(firstLetter) ) { // 第一次添加元素到cityListResult最终结果中
            // 通过judge函数进行判断最终数据中没有的话
			cityListResult.push(
            	{
                    index: firstLetter,
                    list: [{
                        nm: cities[i].nm,
                        id: cities[i].id
                    }]
                }
            )
       	}else { // 不是第一次添加到cityListResult中,则累加index索引
            // 通过judge函数判断最终数组中已经存在了
            for(let j = 0;j < cityListResult.length;j ++) {
                if( cityListResult[j].index === firstLetter ) {
                    // 找到已经有的索引,对应索引里面再次添加一个对象
                    cityListResult[j].list.push({
                        nm: cities[i].nm,
                        id: cities[i].id
                    })
                }
            }
        }
        
        // 封装函数,用以判断结果数组中是否有元素
        // return true 表示最后结果数组中还未存在对应项索引
        function judge(firstLetter) { // firstLetter = A B C D...
            for(let i = 0;i < cityListResult.length;i ++) { // 循环遍历cityListResult
                if(cityListResult[i].index === firstLetter) {
                    // 已经有了
                    return false
                }
            }
            return true
        }
    }
    // console.log(cityListResult)
    
    // 将整理完成的数据排序处理
    cityListResult.sort((n1,n2) => {
        if(n1.index > n2.index) {
            return 1
        }else if(n1.index < n2.index) {
            return -1
        }else {
            return 0
        }
    })
    console.log(cityListResult)
    console.log(hotList)
}
formatCityList(cities)
```





​                                                                                