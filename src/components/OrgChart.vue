<template>
  <div>
    <json-view 
    :data="test"
    rootKey="view"
    :maxDepth="1"
    :styles="{ key: '#0977e6' }"
     />
  </div>
</template>

<script>
  import axios from 'axios'
  import _ from 'lodash'
  import { JSONView } from "vue-json-component";

  export default {
    components: {
        "json-view": JSONView 
    },
    data() {
      return {
        orgInfoList: {},
        orgMemberInfoList: {},
        data:{first : 'jk'},
        test:{
          first: "level",
          works: true,
          object: {
            working: "properly"
          },
          number: 100,
          missing: null,
          list: [
            "fun",
            {
              test: {
                passed: true
              }
            }
          ]
         
        }
    }
    },
    created () {
      axios.get(`http://192.168.165.15:10080/api/orginfo/member/list/OSSTEM/0000`)
      .then((result) => {
        this.data = result.data;
        this.orgInfoList = { orgInfoList : this.data.orgInfoList};
        this.orgMemberInfoList = { orgMemberInfoList : result.data.orgMemberInfoList};
        console.log(result);
        console.log(this.data);
        console.log(this.orgInfoList);
        // JSON.parse(JSON.stringify( temp1.orgInfoList ))
        // _.pullAllBy(temp1.orgInfoList, [{'orgLevel': 3 },{'orgLevel': 4 },{'orgLevel': 2 },{'orgLevel': 1 },{'orgLevel': 0 },{'orgLevel': 5 }], 'orgLevel');
       }).catch((ex)=> {
        console.log("ERR!!!!! : ", ex)
        console.log(_.compact([0,1,true,2,'',3]));
      })
      
    },
    
  }
</script>

<style lang="scss" scoped>

</style>