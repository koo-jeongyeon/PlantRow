<template>
    <v-container>
        <v-row class="text-center">
            <v-col cols="12">
            
            로그인 창
            <div>
                    <v-text-field
                        label="Main input"
                        :rules="rules"
                        hide-details="auto"
                        v-model="homeid"
                    ></v-text-field>
                    <v-text-field 
                        label="Another input"
                        v-model="password"
                    ></v-text-field>

                    <v-btn
                    elevation="2"
                    @click="loginBtn()"
                    >로그인</v-btn>

                    <router-link :to="`/join`">
                         <v-btn
                        elevation="2"
                        >회원가입</v-btn>
                    </router-link>



            </div>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>

export default {
    name: 'LoginView', 

    data: () => ({
        rules: [
        value => !!value || 'Required.',
        value => (value && value.length >= 3) || 'Min 3 characters',
        ],
        
        homeid: "",
        password: "",
    }),

    

    methods: {

        getUserInfo() {
            return this.$axios({
                    method: 'post',
                    url: '/api/login',
                    data: {
                        homeid: this.homeid,
                        password: this.password
                    }
                })
        },
      
        async loginBtn () { 
 
            try {
                const getUserInfoPromise = await this.getUserInfo()
                
                this.$router.push({
                    name: "home",
                    params: {homeid: getUserInfoPromise.data.homeid}
                })

                console.log("dd :" + getUserInfoPromise);

                this.$store.commit("SET_NAME",getUserInfoPromise.data.name)

            } catch (error) {
                console.log(error);
            }
        },

    },
    
}
</script>

<style>
 
</style>
 
