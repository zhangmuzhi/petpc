<template>
    <div class="m-comments">
        <p class="c-title">评论</p>
        <div class="c-p">
            <img :src="userData.head||'/src/assets/avatar_default.png'">
            <textarea ref = 't1' v-model="content1" placeholder="说说你的想法"  ></textarea>
            <div class="c-submit clearfix" >
                <div class="c-btn" @click="setComments">评论</div>
            </div>
            <div class="c-login" v-if="isShowLogin">
                <router-link to="/signin" class="c-btn">登录</router-link><span>后发表评论</span>
            </div>
        </div>
        <div class="c-comments">
            <!--<div class="comments-list">-->
                <!--<img src="http://localhost:3001/user/20170117/file-1484533279305.png">-->
                <!--<p class="c-name">卑劣者</p>-->
                <!--<time>2017.02.09 13:17</time>-->
                <!--<p class="c-content">拍马屁，我只服那一句“笔神落惊风雨，诗成泣鬼”</p>-->
                <!--<span class="c-reply-btn" @click = "showReply('1','',$event)">回复</span>-->
                <!--<ul class="c-ul-list">-->
                    <!--<li>-->
                        <!--<p><a>卑劣者</a>:<span>我只服那一句</span></p>-->
                        <!--<time>2017.02.09 13:17</time> <span class="c-l-reply" @click = "showReply('1','卑劣者',$event)">回复</span>-->
                    <!--</li>-->
                    <!--<li>-->
                        <!--<p><a>卑劣者</a>:<span>我只服那一句</span></p>-->
                        <!--<time>2017.02.09 13:17</time> <span class="c-l-reply" @click = "showReply('1','卑劣者1',$event)">回复</span>-->
                    <!--</li>-->
                <!--</ul>-->
                <!--<div ref="t1" id="1" class="js-reply c-reply content z-none">-->
                    <!--<textarea v-model="content2" :placeholder="replyName"></textarea>-->
                    <!--<a @click="cancel" class="c-btn cancel">取消</a>-->
                    <!--<a class="c-btn sure">确定</a>-->
                <!--</div>-->

            <!--</div>-->
            <!--<div class="comments-list">-->
                <!--<img src="http://localhost:3001/user/20170117/file-1484533279305.png">-->
                <!--<p class="c-name">卑劣者</p>-->
                <!--<time>2017.02.09 13:17</time>-->
                <!--<p class="c-content">拍马屁，我只服那一句“笔神落惊风雨，诗成泣鬼”</p>-->
                <!--<span class="c-reply-btn" @click = "showReply('2','',$event)">回复</span>-->
                <!--<ul class="c-ul-list">-->
                    <!--<li>-->
                        <!--<p><a>卑劣者</a>:<span>我只服那一句</span></p>-->
                        <!--<time>2017.02.09 13:17</time> <span class="c-l-reply" @click = "showReply('2','卑劣者',$event)">回复</span>-->
                    <!--</li>-->
                    <!--<li>-->
                        <!--<p><a>卑劣者</a>:<span>我只服那一句</span></p>-->
                        <!--<time>2017.02.09 13:17</time> <span class="c-l-reply" @click = "showReply('2','卑劣者1',$event)">回复</span>-->
                    <!--</li>-->
                <!--</ul>-->
                <!--<div ref="t2" id="2" class="js-reply c-reply content z-none">-->
                    <!--<textarea v-model="content2" :placeholder="replyName"></textarea>-->
                    <!--<a @click="cancel" class="c-btn cancel">取消</a>-->
                    <!--<a class="c-btn sure">确定</a>-->
                <!--</div>-->

            <!--</div>-->
            <div class="comments-list" v-for="(item,index) in commentsList "  :id ="item._cid" >
                <img :src="item.head">
                <p class="c-name">{{item.name}}</p>
                <time>{{item.newTime}}</time>
                <p class="c-content">{{item.content}}</p>
                <span class="c-reply-btn" @click = "showReply(item._cid,item._uid,item.name,$event)">回复</span>
                <ul class="c-ul-list" >
                <li v-for="(item1,index1) in item.replyContent ">
                <p><a>{{item1.username}}</a>回复<a>{{item1.rname}}</a>:<span>{{item1.content}}</span></p>
                <time>{{item1.newTime}}</time> <span class="c-l-reply" @click = "showReply(item._cid,item1._uid,item1.username,$event)">回复</span>
                </li>

                </ul>
                <div :ref="item.ref"  class="c-reply content z-none">
                    <textarea v-model="content2" :placeholder="replyName"></textarea>
                    <a @click="cancel" class="c-btn cancel">取消</a>
                    <a class="c-btn sure" @click ="submitReply(item._cid,rid,rname)">确定</a>
                </div>
            </div>

        </div>
    </div>
</template>
<style rel="stylesheet/scss" lang = "scss" >
    @import "../css/base/min";
   .m-comments{
       position: relative;
       width: 800px;
       margin: 30px auto;
       padding:20px;
       box-sizing: border-box;
       background-color: #fff;
       border-top:1px solid #f1f1f1;
       .c-login{
           position: absolute;
           width: 100%;
           height: 100%;
           z-index: 5;
           text-align: center;
           top:0;
           left:0;
           a,span{
               position: relative;
               top:45px;
               display: inline-block;
               font-size: 14px;;
           }
           a{
               margin-right: 10px;
           }

       }
       .c-title{
           text-align: center;
           color: #909090;
           font-size: 18px;
           padding: 20px 0;
       }
       .c-submit{
           position: relative;
           width: 100%;
           height: 40px;
           margin-top: 20px;
           left:30px;
           .c-btn{
               position: absolute;
               top:5px;
               right: 0;
           }
       }
       .c-p{
           position: relative;
           width: 600px;
           margin: 0 auto 30px;
           background-color: #f8f9fa;
           border: 1px solid #f1f1f1;
           padding:20px 50px;
           img{
               position: absolute;
               display: block;
               left: 1.5em;
               border-radius: 100%;;
               top: 20px;
               width:40px;
               height: 2.5em;
           }
           textarea{
               left: 30px;
               top:0;
               height: 80px;;
           }

       }
       .c-comments{
            position: relative;
           width: 600px;
           margin:0 auto;
       }
       .comments-list{
           position: relative;
           padding: 20px;
           box-sizing: border-box;
           border-bottom:1px solid #f1f1f1;
           img{
               display: inline-block;
               border-radius: 100%;
               width: 40px;
               height: 40px;
               border:1px solid #f1f1f1;
           }
           .c-name{
               position: absolute;
               display: inline-block;
               color: #666;
               left: 70px;;
           }
           .c-content{
               color: #333;
               line-height: 2;
               padding-top: 10px;;
           }
           time{
               position: absolute;
               left: 70px;;
               top:40px;
               display: inline-block;
               color: #999;
               font-size: 12px;;
           }
           &:hover{
               .c-reply-btn{

               }
           }
           .c-reply-btn{
               position: absolute;
               z-index: 3;
               right: 40px;
               top:30px;
               font-size: 14px;
               color: #999;
               display: block;
               cursor: pointer;
           }
       }
       .c-reply{
           position: relative;
           transition:all .3s;
           margin-bottom: 50px;
           &.z-none{
                display:none;
            }
           textarea{
               margin-top: 10px;
               height: 80px;
           }
           .sure{
                margin-top: 10px;;
               right: 0;
           }
           .cancel{
                background-color: #fff;
               color: $color;
               border: 1px solid $color;
               box-sizing: border-box;
               margin-top: 10px;;
               right: 80px;
           }
           &.z-show{
               display: block;
           }
       }
       textarea{
           position: relative;
           height: 40px;
           border-radius: 4px;
           border: 1px solid #ddd;
           display: block;
           padding: 8px;
           transition: border .3s;
           background: #fff;
           box-sizing: border-box;
           width:100%;
           resize: none;
           &:focus{
               border-color: $color
           }
       }

       .c-ul-list{
           margin: 10px 0;
           border-left: 3px solid #e1e1e1;
           padding-left: 20px;
           .c-l-reply{
               position:relative;
               display: inline-block;
                font-size: 12px;
               color: #999;
               cursor: pointer;
           }
           p{
               color: #666;
               font-size: 14px;

           }
           span{
                padding-left: 5px;;
           }
           a{
               color: #5a7a96;
               padding-right: 5px;
           }
           time{
               color: #999;
               display: inline-block;
               position: relative;
               top:0;
               left:0;
               font-size: 12px;;
           }
           li{
               position:relative;
               padding:10px 0;
               border-bottom:1px dashed #f1f1f1;
           }
       }
   }
</style>
<script>
import util from '../js/util'
    export default{
        data(){
            return{
                replyName:'评论',
                eTarget:'',
                content1:'',
                content2:'',
                userData:{},
                isShowLogin:true,
                lastRef:'',
                lastId:'',
                lastDom:'',
                rid:'',
                rname:''
            }
        },
        created(){
            let vm = this;



            this.$store.dispatch('checkedLogin',function (res) {
                console.log(res.status);
                    if(res.status){
                        vm.isShowLogin = false
                        vm.userData = res.data;
                    }else{
                        vm.isShowLogin = true
                        console.log(vm.$refs.t1);
                        vm.$refs.t1.setAttribute('disabled','disabled');
                        vm.$refs.t1.setAttribute('placeholder','');
                    }
            })

        },

        props:{
            comments:{
                type:Object
            }
        },
        computed:{
            commentsList(){
              if(this.comments.comments){
                  let i = 0,l = this.comments.comments.length;
                   if(l>0){
                       this.comments.comments.map(function(v,k){
                           v.ref = 't'+v._cid;
                           v.newTime = util.changeTime(v.time);
                           let c=0;
                           for(;c< v.replyContent.length;c++){
                               v.replyContent[c].newTime =  util.changeTime(v.replyContent[c].time)
                           }
                       })
                   }
                  console.log(this.comments.comments);
                  return this.comments.comments
              }else{
                  return []
              }

            }
        },
        methods:{
            enter(){

            },
            /*
            *
            * 显示回复框
            * id 回复框id
            * name 回复人昵称
            * */
            showReply(id,rid,rname,e){

                let vm = this;
                let d = 't'+id;
                let c = vm.$refs[d][0].classList;

                vm.rname = rname;
                vm.rid = rid;

                if(this.$store.getters.isLogin==true){

                    if(vm.lastDom == e.target){

                        if(c.value.indexOf('z-none')>=0){
                            c.remove('z-none')
                        }else{
                            c.add('z-none')
                        }
                    }else{
                        if(rname){
                            this.replyName = '对 '+rname+' 的回复'
                        }else{
                            this.replyName = '评论'
                        }
                         if(vm.lastId!=id){
                             if(vm.lastId){
                                 vm.$refs['t'+vm.lastId][0].classList.add('z-none');
                             }
                             c.remove('z-none');
                        }

                    }
                    vm.lastDom = e.target;
                    vm.lastId = id
                }else{
                    this.$router.push('/signin')
                }


            },
            /*取消回复*/
            cancel(e){
                e.target.parentNode.classList.remove('z-show');
            },
            /*评论*/
            setComments(){
                let vm = this;

                console.log(vm.userData);

                if(this.content1==''){
                    vm.$store.commit('SET_TIPS','请输入评论');
                }else{
                    console.log(vm.userData)
                    vm.$store.dispatch('setComments',{
                        _aid:this.$route.query.id,
                        _uid:vm.userData._id,
                        name:vm.userData.username,
                        head:vm.userData.head,
                        content:vm.content1
                    }).then(function(){
                        vm.content1 = ''
                    })
                }
            },
            /*
            * 回复别人
            * cid 评论id
            * */
            submitReply(cid,rid,rname){
                let vm  = this;
                if(this.content2==''){
                    vm.$store.commit('SET_TIPS','请输入内容');
                }else{
                    vm.$store.dispatch('setComments',{
                        _aid:vm.$route.query.id,
                        _cid:cid,
                        _uid:vm.userData._id,
                        name:vm.userData.username,
                        _rid:rid,
                        rname:rname,
                        content:vm.content2
                    }).then(function(){
                        vm.content2 = ''
                    })
                }

            }
        }

    }
</script>
