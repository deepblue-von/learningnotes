### avue学习

```vue
<!-- <script src="src/util/validator.js"></script> -->

<template>
<!--组件下面只能有一个主标签 <basic-container> -->
  <basic-container>
      
      <!--管理对话框-->
    <el-dialog :append-to-body="true" [嵌入的dialog必须添加该属性并赋值为true]
               :close-on-click-modal="false" [通过点击框外退出对话框]
               :modal-append-to-body="false" [是否将遮罩添加在对话框前]
               :visible.sync="manageVisible"[控制对话框的显隐]
               title="管理小区" 
               fullscreen="true"[充满整个屏幕]
               @close="closeManage"[关闭对话框时触发事件]>
        
      <!--使用自定义组件-->
      <residentialManage ref="resMang" 
                         :residentialId="selectedResidentialId" [通过数据绑定将红的的数据传递到该组件中的蓝色对象上]
                         :residentialName="selectedResidentialName"
        :visible="manageVisible">
      </residentialManage>
    </el-dialog>
      
      <!--详情对话框-->
    <el-dialog :append-to-body="true" 
               :close-on-click-modal="false" 
               :modal-append-to-body="false" 
               :visible.sync="detailVisible"
               title="小区详情" 
               width="80%">
      <residential-detail :form="detailForm"></residential-detail>
    </el-dialog>
      
      <!--编辑对话框-->
    <el-dialog :append-to-body="true" 
               :close-on-click-modal="false" 
               :modal-append-to-body="false"  
               :visible.sync="editVisible"
               title="小区编辑" 
               width="80%">
      <avue-form v-model="editForm" :option="option" @submit="submitResidential"></avue-form>
    </el-dialog>
   
      
      <!--表格主题部分-->
    <avue-crud :option="option"[组件配置属性] 
               :table-loading="loading"[表格等待框的控制] 
               :data="data"[显示的数据] :
               page.sync="page"[分页的变量(需要sync修饰符)] 						                          :permission="permissionList" [权限？？？]
               :before-open="beforeOpen"[打开前的回调，用于暂停对话框的打开] 
               v-model="form"[双向数据绑定对话框的数据改动与表格数据绑定在一起] 
               ref="crud"[为表格建立一个引用，并命名为crud]                   
               @row-update="rowUpdate"[编辑数据后触发该事件] 
               @row-save="rowSave"[新增数据后确定触发该事件] 
               @row-del="rowDel"[删除一行数据时触发该事件]
               @search-change="searchChange"[点击搜索后触发该事件] 
               @search-reset="searchReset"[清空搜索触发该事件（搜索旁边那个清空按钮）]                          @selection-change="selectionChange"[当表格第一列的选择框状态发生改变时会触发该事件]                @current-change="currentChange"[切换页面时触发该事件，从第一页切换至第二页]
               @size-change="sizeChange" [每页显示的数据条数发生变化时会触发该事件]
               @refresh-change="refreshChange" [点击刷新按钮触发该事件]
               
               @on-load="onLoad">
        
        <!--自定义表格头部左侧内容-->
      <template slot="menuLeft"> <!--menuleft为自定义表格头部左侧内容所留的插槽-->
        <el-button type="danger"  [一个红色的按钮]
                   size="small" icon="el-icon-delete" plain [按钮的样式]
                   
                   
                   v-if="permission.residential_delete" [??]
                   
                   @click="handleDelete">删除
        </el-button>
      </template>

 <!--自定义了一个插槽-->[暂时不知道用在了哪里 ???]
       <template slot-scope="scope" slot="regionForm">
        <avue-cascader v-model="scope.row.region" :dic="regionDic" key-value="value"></avue-cascader>
               <cy-area-select></cy-area-select>
      </template>

<!--自定义操作栏，用slot="menu"--> <!--每一个组件都应该加一个slot-scope，将作用域限定在组件内部-->
      <template slot="menu" slot-scope="scope">
        <el-button type="text" size="small" icon="el-icon-setting">
          <el-dropdown @command="tip">
            <span class="el-dropdown-link">
              操作<i class="el-icon-arrow-down el-icon--right"> </i>
            </span>
 <!--下拉型的操作按钮，包括三个选项用el-dropdown-menu标签包裹-->             
            <el-dropdown-menu slot="dropdown">
                <!--dropdown,下拉列表-->
              <el-dropdown-item divided [显示分割线]
                                @click.native="detail(scope.row)"
                                [el-dropdown-menu组件未定义click事件，为了使用需要加.native]
                                icon="el-icon-office-building">详情</el-dropdown-item>
              <el-dropdown-item divided @click.native="edit(scope.row)" icon="el-icon-school">编辑</el-dropdown-item>
              <el-dropdown-item divided @click.native="manage(scope.row)" icon="el-icon-s-order">管理</el-dropdown-item>

            </el-dropdown-menu>
              
          </el-dropdown>
        </el-button>
      </template>

    </avue-crud>

  </basic-container>
</template>


<script>
  import {
    getList,
    getDetail,
    add,
    update,
    remove,
    getResidentialList,
    getResidentialDetail,
    updateResidential
  } from "@/api/community/residential";
  import {
    mapGetters
  } from "vuex";
  import request from '@/router/axios';
  import CyAreaSelect from "../../components/cy-area-select/cy-area-select";
  import {
    residentialData
  } from "./data";
  import {
    validatePhone
  } from "@/util/validator"
  import residentialManage from "./residentialManage.vue";
  import residentialDetail from "../../components/residential/residentialDetail.vue";

  export default {
    components: {
      CyAreaSelect,
      residentialManage,
      residentialDetail
    },
    data() {
      return {
        selectedResidentialId: "",
        selectedResidentialName: "",
        manageVisible: false,
        form: {},
        detailForm: {},
        detailVisible: false,
        editForm: {},
        editVisible: false,
        regionDic: [],
        query: {},
        loading: true,
        page: {
          pageSize: 10,
          currentPage: 1,
          total: 0
        },
        selectionList: [],
        option: {
          height: 'auto',
          calcHeight: 60,
          tip: false,
          searchShow: true,
          searchMenuSpan: 6,
          border: true,
          index: true,
          viewBtn: false,
          addBtn: true,
          editBtn: false,
          delBtn: true,
          selection: true,
          dialogClickModal: false,
          menuWidth: 150,
          column: [{
              label: "小区名称",
              overHidden: true,
              width: 130,
              prop: "name",
              row: true,
              search: true,
              rules: [{
                required: true,
                message: "请输入小区名称",
                trigger: "blur"
              }]
            },

            {
              label: "所属社区",
              prop: "agencyId",
              search: true,
              width: 150,
              overHidden: true,
              filterable: true,
              type: "select",
              dicUrl: "/api/agency/agency/list?size=999999999",
              props: {
                label: "agencyName",
                value: "id"
              },
              dicFormatter: (res) => {
                return res.data.records;
              },

              rules: [{
                required: true,
                message: "请输入所属机构名称",
                trigger: "blur"
              }]

            },

            {
              label: "详细地址",
              prop: "address",
              overHidden: true,
            },
            {
              label: "楼栋数量",
              prop: "buildingNumber",
              width: 100,
              display: false,
            },
            {
              label: "单元数量",
              prop: "unitNumber",
              width: 100,
              display: false,
            },
            {
              label: "楼层数量",
              prop: "floorNumber",
              width: 100,
              display: false,
            },
            {
              label: "房间数量",
              prop: "roomNumber",
              width: 100,
              display: false,
            },
            {
              label: "住户数量",
              prop: "userNumber",
              width: 100,
              display: false,
              hide: true,
            },
            {
              label: "已录入人脸住户数",
              prop: "userFaceNumber",
              hide: true,
              width: 120,
              display: false,
            },
            {
              label: "负责人姓名",
              prop: "estatePidName",
              labelWidth: 110,
              hide: true,
              rules: [{
                required: true,
                message: "请输入负责人姓名",
                trigger: "blur"
              }]
            },
            {
              label: "负责人电话",
              prop: "estatePidPhone",
              labelWidth: 110,
              hide: true,
              rules: [{
                required: true,
                message: "请输入负责人电话",
                trigger: "blur"
              }, {
                // validator: validatePhone,
                trigger: "blur"
              }]
            },
            {
              label: "图片",
              prop: "pic",
              type: 'upload',
              hide: true,
              propsHttp: {
                url: 'data'
              },
              listType: 'picture-img',
              span: 24,
              row: true,

              action: "/api/upload/putfile",
              display: true,
              rules: [{
                required: false,
                message: "请输入封面图片",
                trigger: "blur"
              }]
            },
            {
              label: "备注",
              hide: true,
              prop: "remark",
              type: "textarea"
            },
            {
              label: "创建时间",
              hide: true,
              width: 85,
              prop: "createTime",
              type: "datetime",
              valueFormat: "yyyy-MM-dd HH:mm:ss",
              display: false,
            },
            {
              label: "旧平台id",
              prop: "oldId",
              hide: true,
              display: false
            }
          ]
        },
        menuData: {
          list: [],
          form: {
            parentId: 0
          },
          option: {
            column: [{
              label: "父级编码",
              prop: "parentId",
              value: 0,
              disabled: true,
              span: 24
            }, {
              label: "权限名称",
              prop: "name",
              span: 24,
              rules: [{
                required: true,
                message: "请输入权限名称",
                trigger: "blur"
              }]
            }, {
              label: "权限标识",
              prop: "code",
              span: 24,
              rules: [{
                required: true,
                message: "请输入权限标识",
                trigger: "blur"
              }]
            }, {
              label: "类型",
              prop: "state",
              span: 24,
              type: "radio",
              dicData: [{
                label: '系统',
                value: 0
              }, {
                label: '菜单',
                value: 1
              }, {
                label: '按钮',
                value: 2
              }],
              rules: [{
                required: true,
                message: "请输入类型",
                trigger: "blur"
              }]
            }, {
              label: "组件地址",
              prop: "component",
              span: 24
            }, {
              label: "图标",
              prop: "icon",
              span: 24
            }, {
              label: "请求地址",
              prop: "path",
              span: 24
            }, {
              label: "权限描述",
              prop: "des",
              span: 24
            }, {
              label: "排序",
              prop: "order",
              span: 24
            }]
          }
        },
        menuOption: {
          column: [{
            label: "父级编码",
            prop: "parentId",
            disabled: true,
            span: 24
          }, {
            label: "权限名称",
            prop: "name",
            span: 24,
            rules: [{
              required: true,
              message: "请输入权限名称",
              trigger: "blur"
            }]
          }, {
            label: "权限标识",
            prop: "code",
            span: 24,
            rules: [{
              required: true,
              message: "请输入权限标识",
              trigger: "blur"
            }]
          }, {
            label: "类型",
            prop: "state",
            span: 24,
            type: "radio",
            dicData: [{
              label: '系统',
              value: 0
            }, {
              label: '菜单',
              value: 1
            }, {
              label: '按钮',
              value: 2
            }],
            rules: [{
              required: true,
              message: "请输入类型",
              trigger: "blur"
            }]
          }, {
            label: "组件地址",
            prop: "component",
            span: 24
          }, {
            label: "图标",
            prop: "icon",
            span: 24


          }, {
            label: "请求地址",
            prop: "path",
            span: 24
          }, {
            label: "权限描述",
            prop: "des",
            span: 24
          }, {
            label: "排序",
            prop: "order",
            span: 24
          }]
        },
        data: []
      };
    },
    computed: {
      ...mapGetters(["permission"]),
      permissionList() {
        return {
          addBtn: this.vaildData(this.permission.residential_add, false),
          viewBtn: this.vaildData(this.permission.residential_view, false),
          delBtn: this.vaildData(this.permission.residential_delete, false),
          editBtn: this.vaildData(this.permission.residential_edit, false)
        };
      },
      ids() {
        let ids = [];
        this.selectionList.forEach(ele => {
          ids.push(ele.id);
        });
        return ids.join(",");
      }
    },
    mounted() {

    },
    methods: {
      closeManage() {
        this.manageVisible = false;
      },
      manage(row) {
        this.selectedResidentialId = row.id;
        this.selectedResidentialName = row.name;
        console.log("id:" + this.selectedResidentialId + "  name" + this.selectedResidentialName)
        this.manageVisible = true;
        this.$nextTick(() => {
          this.$refs.resMang.init();
        })
      },
      edit(row) {
        this.editForm = row;
        this.editVisible = true;
      },
      detail(row) {
        this.detailForm = row;
        this.detailVisible = true;
      },

      rowSave(row, done, loading) {

        add(row).then(() => {
          this.onLoad(this.page);
          this.$message({
            type: "success",
            message: "操作成功!"
          });
          done();
        }, error => {
          loading();
          window.console.log(error);
        });
      },
      submitResidential(row,done, loading) {
        update(this.editForm).then(() => {
          this.onLoad(this.page);
          this.editVisible = false;
          this.$message({
            type: "success",
            message: "操作成功!"
          });
          done();
        }, error => {
          loading();
          console.log(error);
        });
      },
      rowUpdate(row, index, done, loading) {
        row.isUpdateOrg = false;
        updateResidential(row).then(() => {
          // update(row).then(() => {
          this.onLoad(this.page);
          this.$message({
            type: "success",
            message: "操作成功!"
          });
          done();
        }, error => {
          loading();
          console.log(error);
        });
      },
      rowDel(row) {
        if (row.unitNumber > 0) {
          this.$message.error("请先删除该小区的单元和其他数据")
          return
        }
        this.$confirm("确定将选择数据删除?", {
            confirmButtonText: "确定",
            cancelButtonText: "取消",
            type: "warning"
          })
          .then(() => {
            return remove(row.id);
          })
          .then(() => {
            this.onLoad(this.page);
            this.$message({
              type: "success",
              message: "操作成功!"
            });
          });
      },
      handleDelete() {
        if (this.selectionList.length === 0) {
          this.$message.warning("请选择至少一条数据");
          return;
        }
        this.$confirm("确定将选择数据删除?", {
            confirmButtonText: "确定",
            cancelButtonText: "取消",
            type: "warning"
          })
          .then(() => {
            return remove(this.ids);
          })
          .then(() => {
            this.onLoad(this.page);
            this.$message({
              type: "success",
              message: "操作成功!"
            });
            this.$refs.crud.toggleSelection();
          });
      },
      beforeOpen(done, type) {
        let _this = this;
        if (["edit", "view"].includes(type)) {
          // getResidentialDetail(this.form.id).then(res => {
          //   _this.form = res.data.data;
          //   let region = [];
          //   region.push(_this.form.regionProvince);
          //   region.push(_this.form.regionCity);
          //   region.push(_this.form.regionArea);
          //
          //   _this.form.region = region;
          // });
        }
        done();  //用来关闭对话框
      },
      searchReset() {
        this.query = {};
        this.onLoad(this.page);
      },
      searchChange(params, done) {
        this.query = params;
        this.page.currentPage = 1;
        this.onLoad(this.page, params);
        done();
      },
      selectionChange(list) {
        this.selectionList = list;
      },
      selectionClear() {
        this.selectionList = [];
        this.$refs.crud.toggleSelection();
      },
      currentChange(currentPage) {
        this.page.currentPage = currentPage;
      },
      sizeChange(pageSize) {
        this.page.pageSize = pageSize;
      },
      refreshChange() {
        this.onLoad(this.page, this.query);
      },
      onLoad(page, params = {}) {
        this.loading = true;
        getList(page.currentPage, page.pageSize, Object.assign(params, this.query)).then(res => {
          const data = res.data.data;
          this.page.total = data.total;
          this.data = data.records;
          this.loading = false;
          this.selectionClear();
        });
        // getResidentialList(page.currentPage, page.pageSize, Object.assign(params, this.query)).then(res => {
        //   const data = res.data.data;
        //   this.page.total = Number.parseInt(data.total);
        //   this.data = data.records;
        //   this.loading = false;
        //   this.selectionClear();
        // });


      },

    }
  };
</script>

<style scoped>
  >>>.el-cascader-menu__wrap {
    height: 300px !important;
  }
</style>

```

 #### 边栏树形结构

```vue
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <!--设置边栏宽度和背景颜色-->
        <el-tree :props="treeOption" 
                 :data="treeData"  
                 :expand-on-click-node="false"   
                 @node-click="nodeClick" >
           <span class="custom-tree-node" 
                 slot-scope="{ node, data }">
            <span>{{ node.label }}</span>
           
        <!--鼠标悬浮时激活-->   
            <el-popover   
              placement="right" <!--出现的位置-->
              width="200"
              trigger="hover">
      
        <!--使用具名插槽-->        
              <span slot="reference"> 
                <i class="el-icon-arrow-down"></i> <!--一个向右的箭头图标-->
              </span>
               
              <div style="display: grid">
                <el-link 
                         :underline="false" 
                         type="primary"   [主要链接，链接文字是蓝色的]
                         >添加下一级</el-link>
                <el-link :underline="false" type="primary">修改</el-link>
                <el-link :underline="false" type="danger">删除</el-link>
              </div>
            </el-popover>
          </span>
        </el-tree>
       </el-aside>
```

