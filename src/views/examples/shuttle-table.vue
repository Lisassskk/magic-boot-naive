<template>
    <div style="height: 100%; width: 100%">
        <div style="height: 300px">
            <mb-table ref="queryTableRef" v-bind="queryTableOptions" />
        </div>
        <div style="height: 300px">
            <n-button type="primary" @click="getData">获取数据</n-button>
            <mb-editor-table ref="dataTableRef" v-bind="dataTableOptions" />
        </div>
    </div>
</template>

<script setup>
import { reactive, ref, watch } from 'vue'
import { cloneDeep } from 'lodash-es'

const queryTableRef = ref()
const dataTableRef = ref()

watch(dataTableRef, () => {
    dataTableRef.value.setData([{
        username: '这是第一行',
        id: '1',
        roleId: undefined,
        officeIds: undefined,
        isLogin: 0
    }, {
        username: '这是第二行',
        id: '2',
        roleId: undefined,
        officeIds: undefined,
        isLogin: 0
    }])
})

const queryTableOptions = reactive({
    id: 'user-list',
    url: '/system/user/list',
    page: true,
    selection: true,
    where: {
    },
    cols: [
        {
            field: 'username',
            label: '登录名称',
            style: ''
        },
        {
            field: 'name',
            label: '姓名/昵称'
        },
        {
            field: 'officeName',
            label: '所属机构'
        },
        {
            field: 'roles',
            label: '角色',
            type: 'dynamic'
        },
        {
            field: 'phone',
            label: '手机号'
        },
        {
            field: 'createDate',
            label: '创建时间',
            width: 180
        },
        {
            field: 'isLogin',
            label: '禁止登录'
        },
        {
            label: '操作',
            type: 'buttons',
            width: 140,
            fixed: 'right',
            buttons: [
                {
                    label: '复制',
                    type: 'primary',
                    link: true,
                    click: (row) => {
                        let newRow = cloneDeep(row)
                        // 如果想重复一条数据 这里重新给id赋值
                        // newRow.id = $common.uuid()
                        dataTableRef.value.push(cloneDeep(row))
                    }
                }
            ]
        }
    ]
})
const dataTableOptions = reactive({
    props: {
        url: '/system/user/list',
        where: {
            username: 'admin'
        }
    },
    showNo: false,
    operation: {
        delete: {
            confirm: true
        },
        sub: true,
        same: true
    },
    operationWidth: 300,
    cols: [{
        field: 'username',
        label: '登录名称'
    },{
        field: 'roleId',
        label: '角色',
        component: 'select',
        componentProps: {
            url: "/system/role/all",
            placeholder: "请选择角色",
            multiple: true
        }
    },{
        field: 'officeIds',
        label: '组织机构',
        component: 'tree-select',
        componentProps: {
            url: "/system/user/offices",
            placeholder: "请选择组织机构",
            multiple: true
        }
    },{
        field: 'isLogin',
        label: '禁止登录',
        component: 'switch',
        componentProps: {
            checkedValue: 1,
            uncheckedValue: 0
        },
        showLabel: {
            data: [{value: 0, label: '禁用'},{value: 1, label: '启用'}]
        }
    }]
})

function getData(){
    console.log(dataTableRef.value.getData());
}


</script>
