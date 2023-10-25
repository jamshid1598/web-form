<template>
  <form @submit.prevent="handleSubmit">
    <label for="email">Email</label>
    <input id="email" type="email" v-model="email" v-on:blur="validateEmail" required>
    <div v-if="errors.email" class="errors">
        {{ errors.email }}
    </div>

    <label for="password">Password</label>
    <input id="password" type="password" v-model="password" v-on:blur="validatePassword" required>
    <div v-if="errors.password" class="errors">
        {{ errors.password }}
    </div>

    <label for="role">Role</label>
    <select id="role" v-model="role">
        <option value="developer">Web Developer</option>
        <option value="designer">Web Designer</option>
    </select>

    <label for="tempSkill">Skill</label>
    <input type="text" id="tempSkill" v-model="tempSkill" v-on:keyup.enter="addSkill"
    v-on:focus="btn_type='button'" v-on:blur="btn_type='submit'" >
    <span class="help-text">
        type skill name and press ENTER to add skill
    </span> <br>
    <div class="pill" v-for="skill in skills" v-bind:key="skill" @click="removeSkill">
        <span>{{ skill }}</span>
    </div>

    <div class="terms">
        <input id="terms" v-model="terms" type="checkbox" required>
        <label for="terms">Accept terms and conditions</label>
    </div>

    <div class="privacy">
        <div class="private">
            <input type="checkbox" id="private" value="private-info" v-model="privacies">
            <label for="private">Share private data</label>
        </div>
        <div class="cookies">
            <input type="checkbox" id="cookies" value="cookies" v-model="privacies">
            <label for="cookies">Accept all cookies settings</label>
        </div>
        <div class="other">
            <input type="checkbox" id="others" value="others" v-model="privacies">
            <label for="others">Accept all other ruls</label>
        </div>
    </div>
    <div class="submit">
        <button v-bind:type="btn_type">Create An Account</button>
    </div>
  </form>

  <p class="email">Email: {{ email }}</p>
  <p class="password">Password: {{ password }}</p>
  <p class="role">Role: {{ role }}</p>
  <p class="terms">Terms accepted: {{ terms }}</p>
  <p class="privacy">Privacies: {{ privacies }}</p>
</template>

<script>
export default {
    data() {
        return {
            email: '',
            email_errors: null,
            password: '',
            role: '',
            tempSkill: '',
            skills: [],
            terms: false,
            privacies: [],
            btn_type: 'submit',
            errors: {}
        }
    },
    methods: {
        handleSubmit() {
            if (Object.keys(this.errors).length == 0) {
                console.log('Submited')
                console.log('Email: ', this.email)
                console.log('Password: ', this.password)
                console.log('Role: ', this.role)
                console.log('Skills: ', this.skills)
                console.log('Temrs: ', this.terms)
                console.log('Privacy: ', this.privacies)
            } else {
                console.log(this.errors)
            }

        },
        validateEmail() {
            if (!this.email) {
                this.errors.email = 'Email is required';
            } else if (!/^[^@]+@\w+(\.\w+)+\w$/.test(this.email)) {
                this.errors.email = 'Invalid email';
            } else {
                if (this.errors.email) {
                    delete this.errors.email
                }
            }
        },
        validatePassword() {
            if (this.password.length < 8) {
                this.errors.password = 'Password must be at least 8 characters'
            } else if (this.errors.password) {
                delete this.errors.password
            }
        },
        addSkill(e) {
            var temp_skill = ''
            if (this.tempSkill.length > 1) {
                temp_skill = this.tempSkill.toUpperCase()
                if (!this.skills.includes(temp_skill)) {
                    this.skills.push(temp_skill)
                }
                this.tempSkill = ''
            }
        },
        removeSkill(e) {
            var idx = this.skills.indexOf(e.target.innerText)
            if (idx > -1) {
                this.skills.splice(idx, 1)
            }

            // -> another solution
            // this.skills = this.skills.filter((skill) => {
            //     return skill !== e.target.innerText
            // })
        }
    }
}
</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}
label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.5em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold; 
}
input, select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
}
input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}
.pill {
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}
button {
    background: rgb(49, 160, 208);
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}
.submit {
    text-align: center;
}
.errors {
    color: red;
    margin-top: 10px;
    font-size: 0.3em;
    font-weight: bold;
}
.help-text {
    color: #777;
    font-size: 0.3em;
}
</style>