# FlexVars

Powerful string interpolation tool library

[WebSite](https://zhangfisher.github.io/flexvars/)

## Features

-Supports positional and dictionary interpolation
-Supports multiple error handling mechanisms
-Support for null value processing mechanism
-Support filter chain processing of input variable values
-Support variable prefix suffix
-98%+unit test coverage

## Getting Started

```ts
import {FlexVars} from "flexvars"

const flexvars = new FlexVars({
    filters:{
        currency:{
            args:["prefix","suffix","sign"],          
            default:{prefix:"USD ",suffix:"",sign:"$"}
            next:(value:any,args:Record<string,any>,context:FlexFilterContext)=>{
                return `${args.prefix}${args.sign}${value}${args.suffix}`
            }   
        }
    }
})

const _ = flexvars.replace

console.log(_("hello {}","flexvars"))
// => hello flexvars
console.log(_("I am {}","tom")).toBe("I am tom")
// => I am tom
console.log(_("{ value | currency}",100)).toBe("USD $100"))
// => USD $100
console.log(_("{ value | currency('RMB','￥','元')}",100)).toBe("RMB ￥100元"))// 
// => RMB ￥100元
console.log(_("{ value | currency({prefix:'EUR '',suffix:''',sign:'€'})}",100)).toBe("RMB €100"))
// => EUR €100

flexvars.addFilter({
    name:"add",
    args:["step"],
    default:{step:1},
    next(value:any,args:Record<string,any>,context:FlexFilterContext){
        return parseInt(value)+args.step
    }
})
// call chaining
console.log(_("{ value | add}",100)).toBe("101")
console.log(_("{ value | add|add }",100)).toBe("102")
console.log(_("{ value | add(2)|add(3) }",100)).toBe("105")
console.log(_("{ value | add(2)|add(3)|add(4) }",100)).toBe("109")
```