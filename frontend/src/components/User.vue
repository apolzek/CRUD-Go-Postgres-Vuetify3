<template>
    <v-card class="container pa-5">
        <h3>User</h3>
        <div class="container">
            <form @submit="validateAndSubmit">
                <div v-if="errors.length">
                    <div class="alert alert-danger" v-bind:key="index" v-for="(error, index) in errors">
                        {{ error }}
                    </div>
                </div>
                <fieldset class="form-group">
                    <label>First Name</label>
                    <input type="text" class="form-control" v-model="firstName" />
                </fieldset>
                <fieldset class="form-group">
                    <label>Last Name</label>
                    <input type="text" class="form-control" v-model="lastName" />
                </fieldset>
                <fieldset class="form-group">
                    <label>E-mail</label>
                    <input type="text" class="form-control" v-model="email" />
                </fieldset>
                <button class="btn btn-success" type="submit">Save</button>
            </form>
        </div>
    </v-card>
</template>

  
<script>
import UserDataService from "../service/UserDataService";

export default {
    name: "User",
    data() {
        return {
            firstName: "",
            lastName: "",
            email: "",
            errors: [],
        };
    },
    computed: {
        id() {
            return this.$route.params.id;
        },
    },
    methods: {
        refreshUserDetails() {
            UserDataService.retrieveUser(this.id).then((res) => {
                this.firstName = res.data.firstName;
                this.lastName = res.data.lastName;
                this.email = res.data.email;
            });
        },

        //validate input and submit to database
        validateAndSubmit(e) {
            e.preventDefault();
            this.errors = [];
            if (!this.firstName) {
                this.errors.push("Enter valid values");
            } else if (this.firstName.length < 4) {
                this.errors.push
                    ("Enter atleast 4 characters in First Name");
            }
            if (!this.lastName) {
                this.errors.push("Enter valid values");
            } else if (this.lastName.length < 4) {
                this.errors.push
                    ("Enter atleast 4 characters in Last Name");
            }
            if (this.errors.length == 0) {
                if (this.id == -1) {
                    UserDataService.createUser({
                        firstName: this.firstName,
                        lastName: this.lastName,
                        email: this.email,
                        companyId: "26f06c1c-de33-45b7-8adf-f10c635b8f4d"
                    }).then(() => {
                        this.$router.push("/users");
                    });
                } else {
                    UserDataService.updateUser(this.id, {
                        id: this.id,
                        firstName: this.firstName,
                        lastName: this.lastName,
                        email: this.email,
                        companyId: "26f06c1c-de33-45b7-8adf-f10c635b8f4d"
                    }).then(() => {
                        this.$router.push("/users");
                    });
                }
            }
        },
    },
    created() {
        this.refreshUserDetails();
    },
};
</script>