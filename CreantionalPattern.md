# Creational Pattern
provide object creation mechanisms that increase flexibility and reuse of existing code.
## 1.Simple Factory
Sample code:
```Golang
func simpleFactory(i int) API{
	if i==1{
	return API_1
	}else if i==2{
	return API_2
	}
	return nil
}
```
Parameter -> return different class
<br>
Disadvantage: All in one function. Hard to change when the logic is comlicated.

## 2.Factory Method
use interface for creating objects in a superclass, but subclass can be used to alter the type of objects that will be created.<br>

Sample Code:
```Golang
//Actually simple factory example
package main
type Gun interface{
	setName(name string)
	getName()string 
}
type gun struct{
	name string
}
func (g *gun) setName(name string){
	g.name=name
}
func (g *gun) getName()string{
	return g.name
}
//For a certain gun ak47
type AK47 struct{
	gun
}
func newAK47()Gun{
	return &AK47{
	gun:gun{
		name:"AK47"
		},
	}
}
//Factory
func newGun(gunName string)Gun{
	if gunName=="AK47"{
		return newAK47()
	}
	return nil
}
//For using
func main(){
	ak47:=newAK47()
}
```

## 3.Abstract Factory
Use interface for creating all distinct product but leave the actual product creation to create factory classes.<br>
Client code use different factories.<br>
Sample code:<br>
##### iSportFactory.go:Abstract factory interface
```Golang
package main
import "fmt"

type iSportFactory interface{
	makeShoe() iShoe
	makeShirt() iShirt
}
func getSportFactory(brand string)(iSportFactory,error){
	if brand=="adidas"{
		return &adidas{},nil
	}
	if brand=="nike"[
	return &nike{},nil
	]
	return nil,fmt.Errorf("Wrong brand name")
}
```
##### iShoe.go and iShirt.go:Abstract product
```Golang
//iShoe
package main
type ishoe interface{
	setLogo(logo string)
	getLogo()string
}
type shoe struct{
	logo string
}
func (s *shoe) setLogo(logo string){
	s.logo=logo
}
func (s *shoe) getLogo()string{
	return s.logo
}
```
##### adidas.go: Concrete factory
```Golang
package main
type adidas struct{}
func (a *adidas) makeShoe() iShoe{
	return &adidasShoe{
		shoe:shoe{
			logo:"adidas_shoe",
		},
	}
}

func (a *adias) makeShirt() iShirt{
	return &adidasShirt{
		shirt:shirt{
			logo:"adidas_shirt"
		},
	}
}
```
#### adidasShirt.go and adidasShoe.go: Concrete product
```Golang
type adidasShirt struct{
	shirt
}
type adidasShoe struct{
	shoe
}
```
#### Client Code
```Golang
package main
func main(){
	adidasFactory,_:=getSportFactory("adidas")
	nikeFactory,_:=getSportFactory("nike")

	nikeShoe:=nikeFactory.makeShoe()
	nikeShirt:=nikeFactory.makeShirt()

	adidasShoe:=adidasFactory.makeShoe()
	adidasShirt:=adidasFactory.makeShirt()
}
```
## 4.Builder
Sample Code:
#### Builder related
```Golang
//Interface 
package main
type ibuilder interface{
	setWindowType()
	setDoorType()
	setNumFloor()
	getHouse() house
}
func getBuilder(buildertype string)ibuilder{
	if buildertype=="normal"{
		return &normalBuilder
	}
	if buildertype=="igloo"{
		return &iglooBuilder
	}
	return nil
}
//normalBuilder
type normalBuilder struct{
	windowType string
	doorType string
	floor int
}
func newNormalBuilder() *normalBuilder{
	return &normalBuilder{}
}
func (b *normalBuilder) setWindowType(){
	b.windowType="wooden window"
}
func (b.*normalBuilder) setDoorType(){
	b.doorType="wooden type"
}
func (b *normalBuilder) setNumFloor(){
	b.floor=2
}
func (b *normalBuilder) getHouse()house{
	return house{
		doorType:b.doorType,
		windowType:b.windowType,
		floor:b.floor,
	}
}
type house struct{
	doorType string
	windowType string
	floor int
}
//iglooBuilder is similar
```
#### Director
```Golang
type director struct{
	builder ibuilder
}
func newDirector(b ibuilder)*director{
	return &director{
		builder:b,
	}
}
func (d *director)setBuilder(b ibuilder){
	d.builder=b
}
func (d *director)buildHouse()house{
	d.builder.setDoorType()
	d.builder.setWindowType()
	d.builder.setNumFloor()
	return d.builder.getHouse()
}
```
#### main: Client code
```Golang 
func main(){
	normalBuilder:=getBuilder("normal")
	iglooBuilder:=getBuilder("igloo")
	//For normal builder 
	director:=newDirector(normalBuilder)
	normalalHouse:= director.buildHouse()
	//For igloo builder: new or use director.setBuilder
	director.setBuilder(iglooBuilder)
	iglooHouse:=director.buildHouse()
}
```
## 5.Prototype

## 6.Singleon 
