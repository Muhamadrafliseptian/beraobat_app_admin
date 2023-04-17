<template>
    <div class="row pb-3">
        <div class="col-6 text-start">
            <h4 class="card-title">
                Data Dokter
            </h4>
        </div>
        <div class="col-6 text-end">
            <a href="" class="btn btn-sm btn-primary px-4">Tambah Data +</a>
        </div>
    </div>
    <div class="col-lg-12 grid-margin stretch-card">
        <div class="card">
            <div class="card-body">
                <LoadingComponent v-if="isLoading"   />
                <div class="table-responsive">
                    <table class="table table-striped pb-5">
                        <thead>
                            <tr>
                                <th>
                                    Foto
                                </th>
                                <th>
                                    Nama
                                </th>
                                <th>
                                    Alamat
                                </th>
                                <th>
                                    Nomor Telepon
                                </th>
                                <th>
                                    Status
                                </th>
                                <th class="text-center">
                                    Aksi
                                </th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="perawat in perawats" :key="perawat.id">
                                <td class="py-1">
                                    <img :src="'./assets/images/faces/face1.jpg'" alt="image" />
                                </td>
                                <td>
                                    {{ perawat.getUser.nama }}
                                </td>
                                <td>
                                    {{ perawat.getUser.alamat }}
                                </td>
                                <td>
                                    {{ perawat.getUser.nomorHp }}
                                </td>
                            <td>
                                <label class="switch">
                                    <input type="checkbox" :checked="perawat.getUser.status == 1">
                                    <span class="slider round" @click="updateStatus(perawat.getUser.id)"></span>
                                </label>
                            </td>
                                <td class="text-center">
                                    <router-link to=""
                                        class="btn btn-danger btn-sm me-2 text-white">Delete</router-link>
                                    <router-link to="" class="btn btn-success btn-sm text-white">Detail</router-link>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import LoadingComponent from '@/components/LoadingComponent.vue';
export default {
    data() {
        return {
            perawats: [],
            isLoading: false,
            isEmpty: false,
            status: 0
        }
    },
    created() {
        this.getPerawat()
    },
    methods: {
        getPerawat() {
            this.isLoading = true
            this.isEmpty = true
            const params = [].join("&")
            this.$store.dispatch("getData", ["akun/perawat", params]).then((result) => {
                this.perawats = result.data
                console.log(result.data);
                setTimeout(() => {
                    this.isLoading = false
                    this.isEmpty = false
                }, 1000);
            }).catch((err) => {
                console.log(err);
                this.isEmpty = false
                this.isLoading = false
            });
        },
        updateStatus(id_user) {
            console.log('error');
                const selfEdit = this
                var type = "updateData";
                var url = [
                    "akun/active_account", id_user, {
                    }
                ]
                selfEdit.isLoading = true;
                selfEdit.$store.dispatch(type, url).then((response) => {
                    console.log(response);
                    selfEdit.isLoading = false;
                })
            },
    },
    components: {
        LoadingComponent,
    }
}
</script>

<style>
/* The switch - the box around the slider */
.switch {
    position: relative;
    display: inline-block;
    width: 54px;
    height: 26px;
}

/* Hide default HTML checkbox */
.switch input {
    opacity: 0;
    width: 0;
    height: 0;
}

/* The slider */
.slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    -webkit-transition: .4s;
    transition: .4s;
}

.slider:before {
    position: absolute;
    content: "";
    height: 20px;
    width: 22px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    -webkit-transition: .4s;
    transition: .4s;
}

input:checked+.slider {
    background-color: #2196F3;
}

input:focus+.slider {
    box-shadow: 0 0 1px #2196F3;
}

input:checked+.slider:before {
    -webkit-transform: translateX(26px);
    -ms-transform: translateX(26px);
    transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
    border-radius: 34px;
}

.slider.round:before {
    border-radius: 50%;
}
</style>