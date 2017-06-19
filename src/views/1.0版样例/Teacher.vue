<template>
    <section>
        <!--工具条-->
        <el-col :span="24" class="toolbar" style="padding-bottom: 0px;margin-bottom: 20px">

            <div class="group-box">

                <form>
                    <fieldset style="font-size: 15px;">
                        <legend style="font-weight: 700">查询区</legend>

                        <span style="font-weight: 700">教师编号&nbsp;&nbsp;&nbsp;</span>
                        <el-input type="text" class="form-control" id="idEdit" name="id" placeholder="请输入教师编号"
                                  style="width: 25%"/>
                        　　&nbsp;&nbsp;&nbsp;
                        <span style="font-weight: 700">教师姓名&nbsp;&nbsp;&nbsp;</span>
                        <el-input type="text" class="form-control" id="nameCreate"
                                  name="name" placeholder="请输入教师姓名" style="width: 25%"/>

                        <br><br>

                        <span style="font-weight: 700">电子邮件&nbsp;&nbsp;&nbsp;</span>
                        <el-input type="text" class="form-control" id="emailCreate"
                                  name="email" placeholder="请输入电子邮件" style="width: 25%"/>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

                        <span style="font-weight: 700">工作年限&nbsp;&nbsp;&nbsp;</span>
                        <el-input type="text" class="form-control" id="workTimeCreate"
                                  name="workTime" placeholder="请输入工作年限" style="width: 25%"/>

                        <br>


                        <div class="pull-right">
                            <el-button type="info" id="searchButton">查询</el-button>
                            <el-button id="resetButton">重置</el-button>
                        </div>

                    </fieldset>

                </form>
            </div>
        </el-col>
        <hr style="color: #EEF1F6">


        <el-col highlight-current-row style="margin-top: 15px">
            <el-table-column class="pull-right" style="background-color: #EEF1F6;width:100%;padding: 7px 0;">

                <el-button type="danger" size="middle" class="pull-right" @click="handleDel">删除教师</el-button>

                <el-button size="middle" class="pull-right" @click="handleEdit">编辑教师</el-button>

                <el-button size="middle" class="pull-right" @click="handleAdd">新增教师</el-button>
            </el-table-column>

        </el-col>

        <el-table :data="users" stripe style="width: 100%;">
            <el-table-column type="selection"></el-table-column>
            <el-table-column prop="id" label="教师编号"></el-table-column>
            <el-table-column prop="name" label="教师姓名"></el-table-column>
            <el-table-column prop="birthday" label="出生日期"></el-table-column>
            <el-table-column prop="sex" label="性别"></el-table-column>
            <el-table-column prop="phoneNo" label="联系方式"></el-table-column>
            <el-table-column prop="email" label="电子邮件"></el-table-column>
            <el-table-column prop="workTime" label="工作年限"></el-table-column>
        </el-table>

        <!--工具条-->
        <el-col :span="24" class="toolbar">
            <el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="20"
                           :total="total" style="float:right;">
            </el-pagination>
        </el-col>

        <!--编辑界面-->
        <el-dialog title="编辑教师" v-model="editFormVisible" :close-on-click-modal="false">
            <el-form :model="editForm" label-width="80px" :rules="editFormRules" ref="editForm">
                <el-form-item label="教师编号" prop="id">
                    <el-input v-model="editForm.number" auto-complete="off"></el-input>
                </el-form-item>

                <el-form-item label="教师姓名" prop="name">
                    <el-input v-model="editForm.name"></el-input>
                </el-form-item>

                <el-form-item label="出生日期" prop="birthday">
                    <el-input v-model="editForm.birthday"></el-input>
                </el-form-item>

                <el-form-item label="性别" prop="sex">
                    <el-input v-model="editForm.sex"></el-input>
                </el-form-item>

                <el-form-item label="联系方式" prop="phoneNo">
                    <el-input v-model="editForm.phone"></el-input>
                </el-form-item>

                <el-form-item label="电子邮件" prop="email">
                    <el-input v-model="editForm.email"></el-input>
                </el-form-item>

                <el-form-item label="工作年限" prop="workTime">
                    <el-input v-model="editForm.worktime"></el-input>
                </el-form-item>

            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click.native="editFormVisible = false">取消</el-button>
                <el-button type="primary" @click.native="editSubmit" :loading="editLoading" >提交</el-button>
            </div>
        </el-dialog>

        <!--新增界面-->
        <el-dialog title="新增教师" id="teacherCreateModal" v-model="addFormVisible" :close-on-click-modal="false">
            <el-form id="teacherCreateForm" :model="addForm" label-width="80px" :rules="addFormRules" ref="addForm">
                <el-form-item label="教师姓名" prop="name">
                    <el-input id="nameCreate" v-model="addForm.name" auto-complete="off"
                              placeholder="请输入教师姓名"></el-input>
                </el-form-item>
                <el-form-item label="出生日期" prop="birthday">
                    <el-input id="birthdayCreate" v-model="addForm.birthday" auto-complete="off"
                              placeholder="请输入出生日期"></el-input>
                </el-form-item>
                <el-form-item label="性别">
                    <el-radio-group v-model="addForm.sex" auto-complete="off">
                        <el-radio class="radio" :label="0" id="sexManCreate">男</el-radio>
                        <el-radio class="radio" :label="1" id="sexWomanCreate">女</el-radio>
                    </el-radio-group>
                </el-form-item>
                <el-form-item label="联系方式" prop="phoneNoCreate">
                    <el-input id="phoneNoCreate" v-model="addForm.phone" auto-complete="off"
                              placeholder="请输入联系方式"></el-input>
                </el-form-item>
                <el-form-item label="电子邮件" prop="emailCreate">
                    <el-input id="emailCreate" v-model="addForm.email" auto-complete="off"
                              placeholder="请输入电子邮件"></el-input>
                </el-form-item>
                <el-form-item label="工作年限" prop="workTimeCreate">
                    <el-input id="workTimeCreate" v-model="addForm.worktime" auto-complete="off"
                              placeholder="请输入工作年限"></el-input>
                </el-form-item>

            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click.native="addFormVisible = false">取消</el-button>
                <el-button id="teacherCreateOKButton" type="primary" @click="add" @click.native="addSubmit"
                           :loading="addLoading">提交
                </el-button>
            </div>
        </el-dialog>
    </section>


</template>
<script>
//    import util from '../../common/js/util'

    export default {
        data() {
            return {
                filters: {
                    name: ''
                },
                users: [

                    {
                        "id": "zfe2h5ol1jcpekgvwjdx74sd",
                        "name": "aa",
                        "birthday": "2017-06-05 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "1",
                        "email": "1@1.com",
                        "workTime": "1"
                    },
                    {
                        "id": "jbjytlh8gljxekjvyc5xpzoc",
                        "name": "testTeacher",
                        "birthday": "1995-02-02 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "136999999999",
                        "email": "abc@abchina.com",
                        "workTime": "2"
                    },
                    {
                        "id": "8282e60a-451f-fd4c-0001-23f55a8c2343",
                        "name": "马化腾2",
                        "birthday": "2017-03-06 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "1123323",
                        "email": "myt@qq.com问问",
                        "workTime": "20"
                    },
                    {
                        "id": "6ji8zte72b36ekgukubpbasf",
                        "name": "aaa",
                        "birthday": "2017-06-09 00:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "a",
                        "email": "a@a.a",
                        "workTime": "2"
                    },
                    {
                        "id": "4pyebuwqav3fekgxuyae8eqb",
                        "name": "测试",
                        "birthday": "2017-05-04 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "123",
                        "email": "123@123.com",
                        "workTime": "0"
                    },
                    {
                        "id": "4pyebupi0l23ekguphaw81qu",
                        "name": "测试",
                        "birthday": "2017-05-02 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "123",
                        "email": "123@123.com",
                        "workTime": "0"
                    },
                    {
                        "id": "4f6de709-8c54-9f52-0000-0527d93957fb",
                        "name": "教师",
                        "birthday": "2017-03-01 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "32",
                        "email": "23",
                        "workTime": "23"
                    },
                    {
                        "id": "4a6ce70a-4393-06eb-0006-d4ce3ea21fb7",
                        "name": "志远",
                        "birthday": "1990-01-29 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "12345678901",
                        "email": "111",
                        "workTime": "34"
                    },
                    {
                        "id": "3f11e90a-27b4-0dc4-0002-7c9c8d70d6e6",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e90a-25b5-ac43-0001-a0c460910e49",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e90a-1d7a-6013-0002-80b1f2d04df7",
                        "name": "?\uD840\uDC00?",
                        "birthday": "2017-01-06 00:00:00.0 CST",
                        "sex": "女",
                        "phoneNo": null,
                        "email": null,
                        "workTime": "3"
                    },
                    {
                        "id": "3f11e90a-1abc-2ce5-0001-40c39fab5d61",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e90a-169a-a75c-0001-e73f627e3c69",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e90a-1143-3d0b-0001-e992e4211898",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e90a-0dbb-aedd-0001-e9a454f2f100",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e90a-065b-2c14-0001-e9f18b2953c0",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-fcb1-00f9-0002-7c9c9c42c7ee",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-f5bd-7e20-0001-40c40b16d4e8",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-f399-7cde-0001-e9fb242f3bc0",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-ecc9-6ff9-0002-7c9c770606fc",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-ea8b-1432-0002-7c9c340270be",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-e61f-059e-0004-fc9c0f2912a7",
                        "name": "111111111111111",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": null,
                        "email": null,
                        "workTime": null
                    },
                    {
                        "id": "3f11e909-d791-0793-0002-7c9c562749b6",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-d642-e129-0001-e9fb72fc7eb1",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-d2e9-2d8e-0001-e87ceede03c7",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-d2a2-1f76-0001-e9fb65985bec",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-d1c1-87d8-0001-40c3d89f57c9",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-cd40-cf15-0001-e9a46f763d7a",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-c937-7d69-0001-e9fb09880191",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-c8d4-11e1-0001-a0c488cf75cc",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-c6f1-c6c4-0001-e769f366d3ec",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-c472-459b-0001-e769d8ef2340",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-c090-8139-0001-a0c44ca97096",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-b8e1-10e1-0001-e869132d705f",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-b729-23f9-0001-e9fb4f74504d",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-a5c0-03a8-0001-e9f170d042ad",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "3f11e909-8e7e-0d14-0001-e8693367c409",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "22",
                        "name": "第一个sss",
                        "birthday": "1949-10-01 09:00:00.0 CST",
                        "sex": null,
                        "phoneNo": null,
                        "email": null,
                        "workTime": null
                    },
                    {
                        "id": "21",
                        "name": "初始化",
                        "birthday": "1949-10-01 09:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1db85laqipuoekgvp3iq4pix",
                        "name": "1",
                        "birthday": "1900-01-01 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "1",
                        "email": "1",
                        "workTime": "1"
                    },
                    {
                        "id": "1d11e90a-7449-2357-0004-179fec779b9c",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e90a-71c5-2094-0002-3dc65c69b404",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e90a-6e62-4aa8-0004-179fec763e7a",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e90a-5a99-b444-0004-17a03e6a99a7",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e90a-4ab0-8e8c-0004-17a03e81329a",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e90a-4a50-2e51-0002-3db1119c4d50",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e90a-3d64-8922-0004-17a028d7ea09",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e90a-2655-bef5-0004-17a04e828577",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e90a-0408-af04-0002-3dbb67db0c0d",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-f87d-6deb-0004-17a04ec99038",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-ee90-a76e-0004-17ac0ff958cf",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-edf9-d394-0004-17a008f34bf5",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-e5f2-ecdd-0002-3db0b58621f5",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-d944-ae5e-0004-17a0201c822e",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-d78f-7b55-0004-179fbd7424c8",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-c915-48c5-0002-3db0f9fe18aa",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-c1d6-fee6-0002-3db0e0afb500",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-b399-2676-0002-3db12c0992ed",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-a9f7-109f-0002-3db13c636a26",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-a55c-fd58-0004-179fbd55b73b",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-9016-f8f1-0004-17c09067ac4d",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-8c3d-95fb-0004-17a010cf81f7",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1d11e909-8984-e46f-0002-3dc67a5b1a02",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-7627-5239-0005-8924fb819843",
                        "name": "初始化111",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-6aa2-9bb7-0003-23fdd4b0cf17",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-5ea4-14d3-0003-277c100c7541",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-581e-298a-0005-8924c5028705",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-5717-4ebb-0003-277c969f2bff",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-518d-25e0-0000-ef1075c5d8ee",
                        "name": "43",
                        "birthday": "2016-08-06 00:00:00.0 CST",
                        "sex": "女",
                        "phoneNo": null,
                        "email": null,
                        "workTime": null
                    },
                    {
                        "id": "1b11e90a-49e0-8b58-0003-24000b94c850",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-45aa-c1ee-0002-d4dcfd3f3f50",
                        "name": "李四",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": null,
                        "email": null,
                        "workTime": "10"
                    },
                    {
                        "id": "1b11e90a-3efb-5dce-0004-4c093dd719fd",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-3dfe-73fc-0005-8923dfb3bc4d",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-3889-ada1-0004-5061115369e4",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-2cc8-981b-0003-277c5df0dea9",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-2683-4dbd-0002-d25a75a240a2",
                        "name": "3333",
                        "birthday": "2016-08-06 00:00:00.0 CST",
                        "sex": "女",
                        "phoneNo": null,
                        "email": null,
                        "workTime": null
                    },
                    {
                        "id": "1b11e90a-1205-d561-0003-23ff607146db",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-0d19-8570-0003-23ff979dc922",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-0a59-b149-0003-277ccdbfd15c",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-0822-1031-0000-05690df0aa6a",
                        "name": "d",
                        "birthday": "2016-06-02 00:00:00.0 CST",
                        "sex": "女",
                        "phoneNo": null,
                        "email": null,
                        "workTime": "6"
                    },
                    {
                        "id": "1b11e90a-02fb-228a-0004-63ff7b6c0995",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e90a-0129-5b67-0000-1c41d4fd426e",
                        "name": "w",
                        "birthday": "2016-06-11 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "1",
                        "email": "4",
                        "workTime": "7"
                    },
                    {
                        "id": "1b11e909-fd77-de74-0000-cfac6694480e",
                        "name": "初始化",
                        "birthday": "1949-10-01 09:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e909-fcfc-5985-0004-5060d925af74",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e909-f39a-8263-0005-892409c38644",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e909-f0ed-e277-0000-056cfbcfcc4a",
                        "name": "t",
                        "birthday": "2016-06-02 00:00:00.0 CST",
                        "sex": "女",
                        "phoneNo": null,
                        "email": null,
                        "workTime": "6"
                    },
                    {
                        "id": "1b11e909-e178-1a8a-0005-892487ed0816",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e909-df12-a6e7-0002-d422f773034a",
                        "name": "李四",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": null,
                        "email": null,
                        "workTime": "10"
                    },
                    {
                        "id": "1b11e909-dd78-a667-0000-056b03221b8b",
                        "name": "f",
                        "birthday": "2016-06-02 00:00:00.0 CST",
                        "sex": "女",
                        "phoneNo": null,
                        "email": null,
                        "workTime": "6"
                    },
                    {
                        "id": "1b11e909-948b-48a6-0004-4c0982ec8d33",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e909-8ea8-2b14-0004-50618818c02d",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e909-8aa6-067d-0004-4c09c6fe632e",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e909-84a6-536d-0003-6310c259e6d8",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1b11e909-81a2-f7ee-0004-63ffe2427be4",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-7eb1-d7af-0000-0f97777f3bcd",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-7970-290a-0000-0f97d4e58d65",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-7544-8a75-0000-16545ca907ec",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-7274-e28c-0000-156387bb8ffd",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-6f83-7fa6-0000-1563c0fdef57",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-5e7a-1bf1-0000-165268531b68",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-5b91-1b91-0000-14297d0d202c",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-4ce9-25f9-0000-16527e8edd95",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-44a9-65b5-0000-156406881d45",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-3b21-b382-0000-0f97c4df9d7c",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-383d-6bd0-0000-0f97ab8762b4",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-32ff-bdbb-0000-1652a8aaf387",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-30b3-3144-0000-1429d662f0bf",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-1d39-d6f2-0000-1563f5e6e196",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-03e4-44be-0000-1565a9cbb889",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e90a-014e-b292-0000-0f979110abe0",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e909-fb97-aefc-0000-0f978506f0ed",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1a16e909-e5c4-d59d-0000-14295b669130",
                        "name": "初始化",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "1211e909-afe6-f64e-0000-1c643d031c2f",
                        "name": "第一个",
                        "birthday": "1949-10-04 01:00:00.0 CST",
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "0tc1e2jck7evekgvjuyvw2jb",
                        "name": "qweqw",
                        "birthday": "2006-06-23 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "123424",
                        "email": "234@qq.com",
                        "workTime": "10"
                    },
                    {
                        "id": "0d11e90a-78d2-94f3-0002-3b80df302d99",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "0d11e90a-51df-f085-0000-817b98688fd2",
                        "name": "王老师",
                        "birthday": "2016-07-28 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "123456",
                        "email": "123@qq.com",
                        "workTime": "3"
                    },
                    {
                        "id": "0d11e90a-008c-816f-0002-42d12177f34e",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "0d11e909-d7f1-dd34-0002-42f4c2573912",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "0d11e909-b9bf-f00d-0001-eb10b7d83303",
                        "name": "张老师",
                        "birthday": "1989-10-01 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": null,
                        "email": null,
                        "workTime": null
                    },
                    {
                        "id": "0d11e909-91fd-c86a-0002-3b8bc87049ec",
                        "name": "UP教师",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": "phoneNo",
                        "email": "email",
                        "workTime": "123"
                    },
                    {
                        "id": "0d11e909-8db5-174c-0001-aaccb143cc56",
                        "name": "陈老师",
                        "birthday": "1985-11-01 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": null,
                        "email": null,
                        "workTime": null
                    },
                    {
                        "id": "0c07e909-d2e1-74ae-0002-1cfa6ee37ede",
                        "name": "教室001",
                        "birthday": "1980-10-01 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "126",
                        "email": "123@126.com",
                        "workTime": "12"
                    },
                    {
                        "id": "080be909-96b3-6b83-0000-938a75be9821",
                        "name": "苗三立",
                        "birthday": "2017-02-10 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "111000033",
                        "email": "miaosli@110.com",
                        "workTime": "1"
                    },
                    {
                        "id": "046def0a-42b3-e077-0000-0af90a684026",
                        "name": "test",
                        "birthday": "2016-10-13 00:00:00.0 CST",
                        "sex": "男",
                        "phoneNo": "12345678901",
                        "email": "111@111.com",
                        "workTime": "11"
                    },
                    {
                        "id": "016def0a-03e1-6516-0000-027df5f5115a",
                        "name": "老师1",
                        "birthday": null,
                        "sex": null,
                        "phoneNo": null,
                        "email": null,
                        "workTime": null
                    }
                ],
                total: 0,
                page: 1,
                listLoading: false,
                sels: [],//列表选中列

                editFormVisible: false,//编辑界面是否显示
                editLoading: false,
                editFormRules: {},
                //编辑界面数据
                editForm: {
                    id: '',
                    name: '',
                    birthday: '',
                    sex: '',
                    phoneNo: '',
                    email: '',
                    workTime: ''
                },

                addFormVisible: false,//新增界面是否显示
                addLoading: false,
                addFormRules: {
                    name: [
                        {required: true, message: '请输入姓名', trigger: 'blur'}
                    ]
                },
                //新增界面数据
                addForm: {
                    id: '2',
                    name: '',
                    birthday: '',
                    sex: '',
                    phoneNo: '',
                    email: '',
                    workTime: ''
                }

            }
        },
        methods: {
            //性别显示转换
            formatSex: function (row, column) {
                return row.sex == 1 ? '男' : row.sex == 0 ? '女' : '未知';
            },
            handleCurrentChange(val) {
                this.page = val;
                this.getUsers();
            },
            //获取用户列表
            getUsers() {
                let para = {
                    page: this.page,
                    name: this.filters.name
                };
                this.listLoading = true;
                //NProgress.start();
                getUserListPage(para).then((res) => {
                    this.total = res.data.total;
                    this.users = res.data.users;
                    this.listLoading = false;
                    //NProgress.done();
                });
            },
            //删除
            handleDel: function (index, row) {
                this.$confirm('确认删除该记录吗?', '提示', {
                    type: 'warning'
                }).then(() => {
                    this.listLoading = true;
                    //NProgress.start();
                    let para = {id: row.id};
                    removeUser(para).then((res) => {
                        this.listLoading = false;
                        //NProgress.done();
                        this.$message({
                            message: '删除成功',
                            type: 'success'
                        });
                        this.getUsers();
                    });
                }).catch(() => {

                });
            },
            //显示编辑界面
            handleEdit: function (index, row) {
                this.editFormVisible = true;
                this.editForm = Object.assign({}, row);
            },
            //显示新增界面
            handleAdd: function () {
                this.addFormVisible = true;
                this.addForm = {
                    number: '',
                    name: '',
                    birthday: '',
                    sex: '0',
                    phone: '',
                    email: '',
                    worktime: ''
                };
            },
            //编辑
            editSubmit: function () {
                this.$refs.editForm.validate((valid) => {
                    if (valid) {
                        this.$confirm('确认提交吗？', '提示', {}).then(() => {
                            this.editLoading = true;
                            //NProgress.start();
                            let para = Object.assign({}, this.editForm);
                            para.birth = (!para.birth || para.birth == '') ? '' : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd');
                            editUser(para).then((res) => {
                                this.editLoading = false;
                                //NProgress.done();
                                this.$message({
                                    message: '提交成功',
                                    type: 'success'
                                });
                                this.$refs['editForm'].resetFields();
                                this.editFormVisible = false;
                                this.getUsers();
                            });
                        });
                    }
                });
            },
            //新增
            addSubmit: function () {
                this.$refs.addForm.validate((valid) => {
                    if (valid) {
                        this.$confirm('确认提交吗？', '提示', {}).then(() => {
                            this.addLoading = true;
                            //NProgress.start();
                            let para = Object.assign({}, this.addForm);
                            para.birth = (!para.birth || para.birth == '') ? '' : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd');
                            addUser(para).then((res) => {
                                this.addLoading = false;
                                //NProgress.done();
                                this.$message({
                                    message: '提交成功',
                                    type: 'success',
                                    add: function () {
                                        this.user.push(this.newUser);
                                        // 添加完newPerson对象后，重置newPerson对象
                                        this.newUser = {
                                            number: '',
                                            name: '',
                                            birthday: '',
                                            sex: '0',
                                            phone: '',
                                            email: '',
                                            worktime: ''
                                        };
                                    }
                                })
                                ;
                                this.$refs['addForm'].resetFields();
                                this.addFormVisible = false;
                                this.getUsers();
                            });
                        });
                    }
                });
            },
            selsChange: function (sels) {
                this.sels = sels;
            },
        },
        mounted() {
            this.getUsers();
        }
    }

</script>

<style scoped>

</style>