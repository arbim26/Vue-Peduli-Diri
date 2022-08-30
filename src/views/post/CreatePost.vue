<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>TAMBAH POST</h4>
                        <hr>

                        <form @submit.prevent="store">
                            <div class="form-group">
                                <label for="title" class="font-weight-bold">USER ID</label>
                                <input type="number" class="form-control" v-model="post.user_id" placeholder="User Id">
                                <!-- validation -->
                                <div v-if="validation.user_id" class="mt-2 alert alert-danger">
                                    {{ validation.user_id[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="title" class="font-weight-bold">TANGGAL</label>
                                <input type="date" class="form-control" v-model="post.tanggal" placeholder="Tanggal">
                                <!-- validation -->
                                <div v-if="validation.tanggal" class="mt-2 alert alert-danger">
                                    {{ validation.tanggal[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="content" class="font-weight-bold">WAKTU</label>
                                <input type="time" class="form-control" v-model="post.waktu" placeholder="Waktu">
                                <!-- validation -->
                                <div v-if="validation.waktu" class="mt-2 alert alert-danger">
                                    {{ validation.waktu[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="content" class="font-weight-bold">LOKASI</label>
                                <input type="text" class="form-control" v-model="post.lokasi" placeholder="Lokasi">
                                <!-- validation -->
                                <div v-if="validation.lokasi" class="mt-2 alert alert-danger">
                                    {{ validation.lokasi[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="content" class="font-weight-bold">SUHU</label>
                                <input type="decimal" class="form-control" v-model="post.suhu" placeholder="Suhu">
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
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {

    setup() {

        //state posts
        const post = reactive({
            user_id: '',
            tanggal: '',
            waktu: '',
            lokasi: '',
            suhu: ''
        })

        //state validation
        const validation = ref([])

        //vue router
        const router = useRouter()

        //method store
        function store() {

            let user_id   = post.user_id
            let tanggal   = post.tanggal
            let waktu = post.waktu
            let lokasi = post.lokasi
            let suhu = post.suhu

            axios.post('http://localhost:8000/api/posts', {
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
            store
        }

    }

}
</script>

<style>
    body{
        background: lightgray;
    }
</style>