# Data types

## Các kiểu dữ liệu cơ bản

- string
- number
- nool

## Các kiểu dữ liệu phức tạp hơn

- list(type): danh sách các phần từ (VD: `[1, 2, 3]`), chú ý rằng mọi phần tử phải có cùng một kiểu dữ liệu.
- set(type): list với các phần từ được sắp xếp và không bị lặp lại (`[2, 1, 3, 3] -> [1, 2, 3]`).
- map(type): kiểu dữ liệu `key/value` nhưng các `value` phải có cùng kiểu dữ liệu (`{string: "string", str: "string"}`).
- object: kiểu dữ liệu `key/value` nhưng các `value` có thể có kiểu dữ liệu khác nhau (`{string: "string", bool: true}`).
- tuple: giống như list nhưng các phần tử có thể có kiểu dữ liệu khác nhau (`{0, "string", true}`).

Dưới đây là một vài đoạn code đơn giản về các kiểu dữ liệu nêu trên.

Với các kiểu dữ liệu đơn giản như `string`, `number`, `bool`

```terraform
variable "str_ex" {
  type = string
  default = "str_val"
}

variable "num_ex" {
  type = number
  default = 99
}

variable "bool_ex" {
  type = bool
  default = true
}
```

Với các kiểu dữ liệu phức tạp như: `list`, `set`, `map`, `object`, `tuple`.

```terraform
variable "list_ex" {
  type = list(string)
  default = ["str1", "str2"]
}

variable "set_ex" {
  type = set(string)
  default = ["a", "b", "c"]
}

variable "map_ex" {
  type = map(any)
  default = {
    name = "test",
    age  =  30
  }
}

variable "object_ex" {
  type = object({
    name = string
    age = number
  })
  default = {
    "name" =  "test",
    "age"  =  30
  }
}

variable "tuple_ex" {
  type = tuple([
    object({
      name = string,
      age = number
    }),
    object({
      name = string,
      age = number
    })
  ])
  default = [{
    name = "test_1",
    age = 29
  }, {
    name = "test_2",
    age = 31
  }]
}
```
