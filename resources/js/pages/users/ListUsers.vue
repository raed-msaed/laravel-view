<script setup>
import axios from 'axios';
import { ref, onMounted, reactive } from 'vue';
import { Form, Field, useResetForm } from 'vee-validate';
import { object, string, number, date } from 'yup';

const users = ref([]);

const editing = ref(false);

const formValues = ref();

const form = ref(null);

const getUsers = () => {
    axios.get('/api/users')
        .then((response) => {
            users.value = response.data;
        })
}

const createUserSchema = object({
    name: string().required(),
    email: string().email().required(),
    password: string().required().min(8),
});

const editUserSchema = object({
    name: string().required(),
    email: string().email().required(),
});

const createUser = (values, { resetForm }) => {
    axios.post('/api/users', values)
        .then((response) => {
            users.value.unshift(response.data); /**use push for insert in the botom list or unshift for insert in the top */

            $('#userFormModal').modal('hide');

            resetForm();
        })
};

const addUser = () => {

    editing.value = false;

    $('#userFormModal').modal('show');
}

const editUser = (user) => {

    console.log(user);

    editing.value = true;

    form.value.resetForm();

    $('#userFormModal').modal('show');

    formValues.value = {
        id: user.id,
        name: user.name,
        email: user.email,
    };
};

onMounted(() => {
    getUsers();
});
</script>


<template>
    <div class="content-header">
        <div class="container-fluid">
            <div class="row mb-2">
                <div class="col-sm-6">
                    <h1 class="m-0">Users</h1>
                </div>
                <div class="col-sm-6">
                    <ol class="breadcrumb float-sm-right">
                        <li class="breadcrumb-item"><a href="#">Home</a></li>
                        <li class="breadcrumb-item active">Appointments</li>
                    </ol>
                </div>
            </div>
        </div>
    </div>


    <div class="content">
        <div class="container-fluid">
            <button @click="addUser" type="button" class="mb-2 btn btn-primary">Add New
                User</button>

            <div class="card">
                <div class="card-body">
                    <table class="table table-bordered">
                        <thead>
                            <tr>
                                <th>#</th>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Registred Date</th>
                                <th>Role</th>
                                <th>Options</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(user, index) in users" :key="user.id">
                                <th scope="row">{{ index + 1 }}</th>
                                <td>{{ user.name }}</td>
                                <td>{{ user.email }}</td>
                                <td>-</td>
                                <td>-</td>
                                <td>
                                    <a href="#" @click.prevent="editUser(user)"><i class="fa fa-edit"></i></a>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>


    <!-- Modal userFormModal-->

    <div class="modal fade" id="userFormModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel"
        aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">

                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">
                        <span v-if="editing">Edit User</span>
                        <span v-else>Add New User</span>
                    </h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <Form ref="form" @submit="createUser" :validation-schema="editing ? editUserSchema : createUserSchema"
                    v-slot="{ errors }" :initial-values="formValues">
                    <div class="modal-body">
                        <div class="form-group">
                            <label for="name" class="form-label">Name</label>
                            <Field name="name" type="text" class="form-control" :class="{ 'is-invalid': errors.name }"
                                id="name" aria-describedby="nameHelp" placeholder="Enter full name" />
                            <span class="invalid-feedback">{{ errors.name }}</span>
                        </div>
                        <div class="form-group">
                            <label for="email" class="form-label">Email</label>
                            <Field name="email" type="email" class="form-control" :class="{ 'is-invalid': errors.email }"
                                id="email" aria-describedby="emailHelp" placeholder="Enter Email" />
                            <span class="invalid-feedback">{{ errors.email }}</span>
                        </div>

                        <div class="form-group">
                            <label for="password" class="form-label">Password</label>
                            <Field name="password" type="password" class="form-control"
                                :class="{ 'is-invalid': errors.password }" id="password" aria-describedby="passwordHelp"
                                placeholder="Enter password" autocomplete="" />
                            <span class="invalid-feedback">{{ errors.password }}</span>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="submit" class="btn btn-primary">Save</button>
                    </div>
                </Form>
            </div>
        </div>
    </div>
</template>