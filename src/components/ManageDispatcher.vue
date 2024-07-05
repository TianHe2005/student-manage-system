<template>
    <div>
        <div class="header">
            收费方式明细
        </div>
        <div class="body">
            <el-table :data="tableData" style="width: 89%" class="table">
                <el-table-column prop="dispatcher_id" label="缴纳金额" width="200" align="center">
                </el-table-column>
                <el-table-column prop="dispatcher_name" label="缴纳方式" width="200" align="center">
                </el-table-column>
                <el-table-column prop="dispatcher_phone" label="人数" width="200" align="center">
                </el-table-column>
                <el-table-column prop="operate" label="操作" width="200" align="center">
                    <template slot-scope="scope">
                        <el-button size="small" type="danger" @click="showdia_dlt(scope.row)">删除
                        </el-button>
                    </template>
                </el-table-column>
                <el-table-column width="120" align="center">
                    <template slot="header">
                        <el-button icon="el-icon-plus" size="small" type="success" @click="showdia_add()">添加收费方式
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>

            <el-dialog title="添加收费方式" :visible.sync="dia_add" width="30%">
                <el-form ref="add_form" :model="add_form" label-width="120px" :rules="add_form_rules">
                    <el-form-item label="缴纳金额：" prop="dispatcher_id">
                        <el-input v-model="add_form.dispatcher_id"></el-input>
                    </el-form-item>
                    <el-form-item label="收费方式：" prop="dispatcher_name">
                        <el-input v-model="add_form.dispatcher_name"></el-input>
                    </el-form-item>
                    <el-form-item label="人数：" prop="dispatcher_phone">
                        <el-input v-model="add_form.dispatcher_phone"></el-input>
                    </el-form-item>
                </el-form>
                <div style="text-align: center;">
                    <el-button type="primary" @click="adddispatcher()">
                        添加
                    </el-button>
                </div>
            </el-dialog>


            <el-dialog title="删除" :visible.sync="dia_dlt" width="30%">
                <div>
                    确定删除此缴费方式吗？
                </div>
                <div style="text-align: center;">
                    <el-button type="primary" @click="deletedispatcher()">
                        确定
                    </el-button>
                </div>
            </el-dialog>
        </div>
    </div>
</template>

<script>
export default {
    created() {
        this.getdata()
    },
    data() {
        return {
            tableData: [],
            dia_add: false,
            dia_dlt: false,
            add_form: {
                dispatcher_id: '',
                dispatcher_name: '',
                dispatcher_phone: '',
            },
            want_delete: '',
            add_form_rules: {
                dispatcher_id: [{ required: true, message: '必填项', trigger: 'blur' }],
                dispatcher_name: [{ required: true, message: '必填项', trigger: 'blur' }],
                dispatcher_phone: [{ required: true, message: '必填项', trigger: 'blur' }]

            },

        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/manager/dispatcher").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia_add() {
            this.dia_add = true;
        },
        adddispatcher() {
            this.$refs.add_form.validate(valid => {
                if (!valid)
                    return;
                else //验证通过再发送请求
                    this.$axios.post("/api/manager/dispatcher", this.add_form).then((res) => {
                        console.log(res.data);
                        if (res.data.status == 200) {
                            this.$message({
                                message: "添加成功",
                                type: "success"
                            })
                            this.dia_add = false;
                            this.getdata();
                        } else {
                            this.$message({
                                message: res.data.msg,
                                type: "error"
                            })
                        }
                    })
            })


        },
        showdia_dlt(row) {
            this.want_delete = row.dispatcher_id;
            this.dia_dlt = true;
        },
        deletedispatcher() {
            this.$axios.delete("/api/manager/dispatcher", { data: { want_delete: this.want_delete } }).then((res) => {
                if (res.data.status == 200) {
                    this.$message({
                        message: res.data.msg,
                        type: "success"
                    })
                    this.getdata()
                    this.dia_dlt = false;
                }
            })
        }
    }
}
</script>

<style scoped>
.header {
    width: 100%;
    height: 10%;
    text-align: center;
    line-height: 64px;
    font-size: 20px;
    font-weight: 800;
    border-bottom: 1px solid #e3e3e3;
}

.body {

    width: 80%;
    margin: auto;
    margin-top: 30px;
}
</style>