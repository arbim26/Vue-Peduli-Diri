<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>EDIT CATATAN</h4>
                        <hr>

                        <form @submit.prevent="update">
                            <div class="form-group">
                                <label for="tanggal" class="font-weight-bold">Tanggal</label>
                                <input type="number" class="form-control" v-model="post.user_id">
                                <!-- validation -->
                                <div v-if="validation.user_id" class="mt-2 alert alert-danger">
                                    {{ validation.user_id[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="tanggal" class="font-weight-bold">Tanggal</label>
                                <input type="date" class="form-control" v-model="post.tanggal">
                                <!-- validation -->
                                <div v-if="validation.tanggal" class="mt-2 alert alert-danger">
                                    {{ validation.tanggal[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="jam" class="font-weight-bold">Waktu</label>
                                <input type="time" class="form-control" v-model="post.Waktu">
                                <!-- validation -->
                                <div v-if="validation.Waktu" class="mt-2 alert alert-danger">
                                    {{ validation.Waktu[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="lokasi" class="font-weight-bold">Lokasi</label>
                                <input type="text" class="form-control" v-model="post.lokasi">
                                <!-- validation -->
                                <div v-if="validation.lokasi" class="mt-2 alert alert-danger">
                                    {{ validation.lokasi[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="suhu" class="font-weight-bold">Suhu</label>
                                <input type="text" class="form-control" v-model="post.suhu">
                                <!-- validation -->
                                <div v-if="validation.suhu" class="mt-2 alert alert-danger">
                                    {{ validation.suhu[0] }}
                                </div>
                            </div>
                            <button type="submit" class="btn btn-primary">SIMPAN</button>
                        </form>                        

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import axios from 'axios'
export default {
    setup() {
        //state posts
        const post = reactive({
            tanggal: '',
            waktu: '',
            lokasi: '',
            suhu: ''
        })
        //state validation
        const validation = ref([])
        //vue router
        const router = useRouter()
        //vue router
        const route = useRoute()
        //mounted
        onMounted(() => {
            //get API from Laravel Backend
            axios.get(`http://127.0.0.1:8000/api/posts/${route.params.id}`)
            .then(response => {
            
            //assign state posts with response data
            post.user_id    = response.data.data.user_id  
            post.tanggal    = response.data.data.tanggal  
            post.waktu  = response.data.data.waktu
            post.lokasi  = response.data.data.lokasi  
            post.suhu  = response.data.data.suhu  
            }).catch(error => {
                console.log(error.response.data)
            })
        })
        //method update
        function update() {
            let user_id   = post.user_id
            let tanggal   = post.tanggal
            let waktu = post.waktu
            let lokasi = post.lokasi
            let suhu = post.suhu
            axios.put(`http://127.0.0.1:8000/api/posts/${route.params.id}`, {
                user_id: user_id,
                tanggal: tanggal,
                waktu: waktu,
                lokasi: lokasi,
                suhu: suhu
            }).then(() => {
                //redirect ke post index
                router.push({
                    name: 'post.index'
                })
            }).catch(error => {
                //assign state validation with error 
                validation.value = error.response.data
            })
        }
        //return
        return {
            post,
            validation,
            router,
            update
        }
    }
}
</script>
Footer
© 2022 GitHub, Inc.