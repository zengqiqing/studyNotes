pinia与vuex创建store的对比

```javascript
//pinia
import {defineStore} from 'pinia'

export const goodStore = defineStore({
  state:()=>{
     return {
       count:1,
       phone:15900000001
     }
  }
    getters:{
			phoneHidden(state){
				return state.phone.toString().replace(/^(\d{3})\d{4}(\d{4})$/, '$1****$2')
			}
		},
  actions:{
    changState(){
      this.count++
    }
  }
})

//vuex
import {createStore} from 'vuex'
const goodStore = createStore({
  state:{
    count:1
  }
  
  getters:{
		getCount(state){return state.count}
	}
})
```

vuex在业务文件中直接通过compued的set方法中修改store的内容是不能修改后立即更新store的文件的，只能通过mutation写逻辑，在业务文件通过this.$store.commit('store文件/方法', {val})





如何理解vuex里的嵌套式结构？

vuex 里面的state会对应到我们组件里的data, getters会对应到组件里的computed;而mutations,action更像是对应到组件里的methods,vuex还有module也就是他有嵌套式的结构，而pinia的state对应到data，getter对应到computed,actions就对应到methods,相比之下少了mutation这一层。并且没了vuex里的嵌套式，降低了耦合度。