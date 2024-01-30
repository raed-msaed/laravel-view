<script setup>
import axios from 'axios';
import { ref, onMounted, reactive } from 'vue';
import { Form, Field } from 'vee-validate';

const users = ref([]);
/*[
    {
        id: 1,
        name: 'John Doe',
        email: 'John@example.com',
    },
    {
        id: 2,
        name: 'Msaed RAED',
        email: 'Raed@example.com',
    },
    {
        id: 3,
        name: 'Darine Dann',
        email: 'Darine@example.com',
    }

];*/

const form = reactive({
    name: '',
    email: '',
    password: '',
});

const getUsers = () => {
    axios.get('/api/users')
        .then((response) => {
            users.value = response.data;
        })
}

const createUser = (values) => {
    console.log(values);
};

// const createUser = () => {
//     axios.post('/api/users', form)
//         .then((response) => {
//             users.value.unshift(response.data); /**use push for insert in the botom list or unshift for insert in the top */
//             form.name = '';
//             form.email = '';
//             form.password = '';
//             $('#createUserModal').modal('hide');
//         });
// };

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
            <button type="button" class="mb-2 btn btn-primary" data-toggle="modal" data-target="#CreateUserModal">Add New
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
                                <td>-</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>


    <!-- Modal CreateUserModal-->
    <div class="modal fade" id="CreateUserModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                    <button type="button" class="close" data-bs-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <Form @submit="createUser">
                    <div class="modal-body">
                        <div class="form-group">
                            <label for="name" class="form-label">Name</label>
                            <Field name="name" type="text" class="form-control" id="name" aria-describedby="nameHelp"
                                placeholder="Enter full name" />
                        </div>
                        <div class="form-group">
                            <label for="email" class="form-label">Email</label>
                            <Field name="email" type="email" class="form-control" id="email" aria-describedby="emailHelp"
                                placeholder="Enter Email" />
                        </div>

                        <div class="form-group">
                            <label for="password" class="form-label">Password</label>
                            <Field name="password" type="password" class="form-control" id="password"
                                aria-describedby="passwordHelp" placeholder="Enter password" />
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