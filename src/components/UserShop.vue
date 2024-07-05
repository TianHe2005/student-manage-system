<template>
    <div>
        <div class="header">
            学生缴费
        </div>
        <div class="body">
            <el-table :data="tableData" style="width: 100%" class="table" border>
                <el-table-column prop="shop_name" label="年级" width="200" align="center">
                </el-table-column>
                <el-table-column prop="price" label="应缴费用" width="200" align="center">
                </el-table-column>
                <el-table-column prop="sale" label="已缴人数" width="200" align="center">
                </el-table-column>
                <el-table-column prop="operate" label="操作" width="208" align="center">
                    <template slot-scope="scope">
                        <el-button icon="el-icon-plus" size="small" type="success" @click="showdia(scope.row)">支付
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>

            <el-dialog title="学生缴费" :visible.sync="dialog" class="dialog" width="40%">
                <div>
                    <el-form ref="form" :model="form" label-width="100px">
                        <el-form-item label="年级：">
                            <span>{{ form.shop_name }}</span>
                            <!-- <el-input v-model="form.shop_name"></el-input> -->
                        </el-form-item>

                        <el-form-item label="应缴费用：">
                            <span>{{ form.order_money }}</span>
                            <!-- <el-input v-model="form.order_money"></el-input> -->
                        </el-form-item>

                        <el-form-item label="缴费方式：">
                            <el-select v-model="form.order_way" placeholder="请选择缴费方式">
                                <el-option label="现金" value="现金"></el-option>
                                <el-option label="刷卡" value="刷卡"></el-option>
                            </el-select>
                        </el-form-item>

                        <!-- <el-form-item label="客户电话：">
                            <el-input v-model="form.cons_phone"></el-input>
                        </el-form-item> -->

                        <el-form-item label="学生姓名：">
                            <el-input v-model="form.cons_name"></el-input>
                        </el-form-item>

                        <el-form-item label="学生学号：">
                            <el-input v-model="form.cons_addre"></el-input>
                        </el-form-item>

                    </el-form>
                    <div style="text-align: center;">
                        <el-button type="primary" @click="add">
                            提交
                        </el-button>
                    </div>
                </div>
            </el-dialog>
        </div>
    </div>
</template>

<script>
export default {
    created() {
        this.getdata();
    },
    data() {
        return {
            tableData: [],
            dialog: false,
            form: {
                shop_name: '',
                order_money: '',
                order_way: '',
                // cons_phone: '',
                cons_name: '',
                cons_addre: '',
            }
        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/user/shop").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia(row) {
            this.form.shop_name = row.shop_name;
            this.form.order_money = row.price;
            this.dialog = true;
        },
        add() {
            this.$axios.post("/api/user/addorder", this.form).then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.$message({
                        message: "成功缴费",
                        type: "success"
                    })
                    this.dialog = false;
                    this.getdata();
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
    width: 62%;
    margin: auto;
    margin-top: 30px;
}

/* .table {
    margin-left: 120px;
} */

.dialog {
    /* width: 700px; */
}
</style>