# ad
autodocumentation Go code process

### Present design

```golang
func main(){
	var a,b int

	// input data
	a = 9+1
	b = 32

	// res - find the result
	var res int
	res = a + b

	for i:=0;i<2;i++{
		res++
	}

	res += rep()

	_ = res
}

// rep function return const
func rep() int {
	r := 10
	return 42 + r
}
```

If run `go run main.go`, then stdout return nothing.

### Proposal

```golang
func main(){
	var a,b int

	// input data
	label("// input data")
	a = 9+1
	output("a", a)
	b = 32
	output("b", b)

	// res - find the result
	label("// res - find the result")
	var res int
	res = a + b
	output("res", res)

	addTab()
	for iter :=0; iter<2 ;iter++{
		output("iter", iter)
		res++
		output("res", res)
	}
	removeTab();

	res += rep()
	output("res", res)

	_ = res
}

// rep function return const
func rep() int {
	label("// rep function return const")
	label("Function rep: run")
	defer func(){
		label("Function rep: stop")
	}()
	r := 10
	output("r", r)
	return 42 + r
}

var tab string

func addTab() {
	tab += "\t"
}

func removeTab() {
	tab = tab[:len(tab)-1]
}

func label(l string) {
	fmt.Println(l)
}

func output(name string, value interface{}){
	fmt.Println(tab, name , " = ", value)
}
```

```
// input data:
a = 10
b = 32
// res - find the result
res = 42
	iter = 0
	res = 43
	iter = 1
	res = 44
// rep function return const
Function rep: run
r = 52
Function rep: stop
res = 96
```
