## 结构体字段标签及其含义示例
> 本示例基于结构体转换成json数据格式 json.Marshal()
>  
> 代码示例中，没有写明结果体时，则为下面的默认机构体

``` 初始结构体
type Road struct { // 默认结构体
		Name   string `json:"name"`
		Number int    `json:"number,omitempty"`
		Value  string `json:"value,omitempty"`
		Open   bool   `json:"open,omitempty"`
	}
```



### \`json:"name"\`
> json.Marshal() 转换成的json数据格式中，该字段值的名称为结构体标签名称，如果结构体中该字段没有赋值，则默认为该字段类型的零值[^零值]

### \`json:"name,omitempty"\`
> 该字段为空，或者赋值为该字段类型的零值时，忽略该字段
>
```
// Rode赋值字段
roads.Name = "Diamond Fork"
roads.Number = 11
// roads.Value = "value1"
roads.Open = false
{
	"name": "Diamond Fork", // json.Marshal() 结果
	"Number": 11
}
Value字段没有赋值、Open 赋值为零值。
```

### \`json:",omitempty"\`
> 该字段为空时忽略，如果不为空，该字段值即为json转换后的字段值【Name->Name】

```
// Rode赋值字段
roads.Name = "Diamond Fork"
roads.Number = 11
{
	"name": "Diamond Fork", // json.Marshal() 结果
	"Number": 11
}
Number字段对应json字段值
```

###  \`json:"-"\`
> 该字段忽略

```
type Road struct {
		Name   string `json:"name"`
		Number int    `json:",omitempty"`
		Value  string `json:"-"` //
}

// 赋值
roads.Name = "Diamond Fork"
roads.Number = 11
roads.Value = "value"
	
{
	"name": "Diamond Fork", // json.Marshal() 结果
	"Number": 11
}
	
```

### \`json:"-,"\`
> 该字段对应json字段名称为“-”

```
type Road struct {
		Name   string `json:"name"`
		Number int    `json:",omitempty"`
		Value  string `json:"-,"` //
}

// 赋值
roads.Name = "Diamond Fork"
roads.Number = 11
roads.Value = "value"

{
	"name": "Diamond Fork",
	"Number": 11,
	"-": "value"
}
```


---
⚠️注意：结构体对应tags名称忽略大小写，即 Name   string `json:"name"` 可正常赋值到RespName   string `json:"Name"`

``` 结果结构体

json:{RespName:"Diamond Fork", RespNumber:11, Value:"value", Open:true}

json.Unmarshal() 转换到结构体 【字段标签忽略大小写】


type RespRode struct { // 结果结构体
		RespName   string `json:"Name"`
		RespNumber int    `json:"Number"`
		Value      string `json:"VALUE"`
		Open       bool   `json:"open"`
}
等同于：
type RespRode struct { // 结果结构体
	RespName   string `json:"name"`
	RespNumber int    `json:"number"`
	Value      string `json:"value"`
	Open       bool   `json:"open"`
}
```




[^零值]: **bool:false**;**integer:0**;**float:0.0**;**string:""**;**pointer、function、interface、slice、channel、map:nil**;对于复合类型，go语言会自动递归地将每一个元素初始化为其类型对应的零值


