<template>
  <div class="" id="app">
    <div class="container">
      <div class="row">
        <div class="col-sm-8 offset-sm-2">
          <div>
            <h2>Change password</h2>
            <div class="alert alert-danger alert-dismissible" v-if="error">
                Old password does not match
                  <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                   </button>
            </div>
            <form @submit.prevent="handleSubmit">
                <div class="form-group">
                <label for="oldPassword">Old Password</label>
                <input
                  type="password"
                  v-model="user.oldPassword"
                  id="oldPassword"
                  name="oldPassword"
                  class="form-control"
                  :class="{
                    'is-invalid': submitted && $v.user.oldPassword.$error,
                  }"
                />
                <div
                  v-if="submitted && $v.user.oldPassword.$error"
                  class="invalid-feedback"
                >
                  <span v-if="!$v.user.oldPassword.required"
                    >Password is required</span
                  >
                  <span v-if="!$v.user.oldPassword.minLength"
                    >Password must be at least 6 characters</span
                  >
                </div>
              </div>
              
              <div class="form-group">
                <label for="newPassword">New Password</label>
                <input
                  type="password"
                  v-model="user.newPassword"
                  id="newPassword"
                  name="newPassword"
                  class="form-control"
                  :class="{
                    'is-invalid': submitted && $v.user.newPassword.$error,
                  }"
                />
                <div
                  v-if="submitted && $v.user.newPassword.$error"
                  class="invalid-feedback"
                >
                  <span v-if="!$v.user.newPassword.required"
                    >Password is required</span
                  >
                  <span v-if="!$v.user.newPassword.minLength"
                    >Password must be at least 6 characters</span
                  >
                </div>
              </div>
              <div class="form-group">
                <label for="confirmPassword">Confirm Password</label>
                <input
                  type="password"
                  v-model="user.confirmPassword"
                  id="confirm_password"
                  name="confirmPassword"
                  class="form-control"
                  :class="{
                    'is-invalid': submitted && $v.user.confirmPassword.$error,
                  }"
                />
                <div
                  v-if="submitted && $v.user.confirmPassword.$error"
                  class="invalid-feedback"
                >
                  <span v-if="!$v.user.confirmPassword.required"
                    >Confirm Password is required</span
                  >
                  <span v-else-if="!$v.user.confirmPassword.sameAsPassword"
                    >Passwords must match</span
                  >
                </div>
              </div>
              <div class="form-group">
                <button class="btn btn-primary">Change Password</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
      </div>
  
</template>

<script>
import { required,minLength, sameAs } from "vuelidate/lib/validators";
 import {changePassword} from '@/common/Service';
export default {
  name: "Changepassword",

  data() {
    return {
      user: {
        oldPassword:"",
        newPassword: "",
        confirmPassword: "",
      },
      error:false,
      submitted: false,
    };
  },
  validations: {
    user: {
      oldPassword: { required, minLength: minLength(6)},
      newPassword: { required, minLength: minLength(6) },
      confirmPassword: { required, sameAsPassword: sameAs("newPassword") },
    },
  },
  methods: {
    handleSubmit() {
      this.submitted = true;
         let formData={old_password:this.user.oldPassword,new_password:this.user.newPassword,confirm_password:this.user.confirmPassword};

      // stop here if form is invalid
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }

      // alert("SUCCESS!! :-)\n\n" + JSON.stringify(this.user));
   console.log(formData);
        changePassword(formData)
      .then(res=>{
             if(res.data.status==1){
                this.$router.push("/");
               console.log(this.formData);
               console.log(res.data);
            }
            else{
               alert("old password does not match")  
                console.log(res.data.message);
            }
        })
        .catch(error=>{
            console.log(error);
            this.error=true;
        })
    },
     
  },
};
</script>
 
<style>
</style>