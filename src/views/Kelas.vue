<template>
  <div class="row">
        <div class="col">
          <div class="card">
            <div v-if="kelas.length == 0">
              <div class="card-header">
                <div class="col" style="text-align:center;  margin-bottom:50px; ">
                  <!-- <div style="height: 100%; display:block; padding:auto 0"> -->
                    <img src="../../public/assets/img/theme/vue.jpg" alt="" srcset="">
                    <p>Ups. Belum ada data kelas nih..</p>
                    <button class="btn btn-success" @click="createKelas" data-toggle="modal" data-target="#modalKelas"><span class="alert-icon"><i class="ni ni-fat-add"></i></span>Kelas</button>
                  <!-- </div> -->
                </div>
              </div>
            </div>
            <div v-else>
              <div v-if="kelas == ''">
              <content-loader
                  viewBox="0 0 476 124"
                  primaryColor="#f3f3f3"
                  secondaryColor="#cccccc"
                >
                  <rect x="10" y="8" rx="3" ry="3" width="65" height="6" />
                  <rect x="10" y="26" rx="3" ry="3" width="52" height="6" />
                  <rect x="20" y="56" rx="3" ry="3" width="300" height="6" />
                  <rect x="20" y="72" rx="3" ry="3" width="300" height="6" />
                  <rect x="20" y="88" rx="3" ry="3" width="178" height="6" />
                </content-loader>
            </div>
            <!-- Card header -->
            <div class="card-header border-0">
              <div class="alert alert-dismissible fade show" :class="alertVariant" role="alert" v-show="alert">
                <!-- <span class="alert-icon"><i class="ni ni-like-2"></i></span> -->
                <span class="alert-text"><strong>{{alertMessage}}</strong></span>
                <button type="button" class="close" data-dismiss="alert" aria-label="Close" @click="!alert">
                    <span aria-hidden="true">&times;</span>
                </button>
              </div>
              <button class="btn btn-success" @click="createKelas" data-toggle="modal" data-target="#modalKelas"><span class="alert-icon"><i class="ni ni-fat-add"></i></span>Kelas</button>
            </div>
            <!-- Light table -->
            <div class="table-responsive">
              <table class="table align-items-center table-flush">
                <thead class="thead-light">
                  <tr>
                      <th></th>
                    <th scope="col">No.</th>
                    <th scope="col">Nama Kelas</th>
                    <th scope="col">Kompetensi Keahlian</th>
                    <th scope="col">Aksi</th>
                  </tr>
                </thead>
                <tbody class="list">
                  <tr v-for="(kelas, index) in kelas" :key="index">
                    <td></td>
                    <td>
                        {{index+1}}
                    </td>
                    <td>
                      {{kelas.nama_kelas}}
                    </td>
                    <td>
                      {{kelas.kompetensi_keahlian}}
                    </td>
                    <td>
                        <button class="btn btn-success" data-toggle="modal" :data-target="'#modaldetailKelas'+index" @click="getDetailKelas(index)">Detail</button>
                        <button class="btn btn-primary" @click="updateKelas(index)" data-toggle="modal" data-target="#modalKelas" >Edit</button>
                        <button class="btn btn-danger" @click="deleteKelas(index)">Delete</button>
                    </td>

        <!-- MODAL DETAIL -->
      <div class="modal fade" :id="'modaldetailKelas'+index"  tabindex="-1" role="dialog" aria-labelledby="modalDetailKelas" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="modalDetailKelas"><span v-if="action === 'create'">Tambah</span> <span v-else>Edit</span> Kelas</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form> 
                <div class="col">
                  <div class="form-group">
                    <div class="row">
                      <label class="form-control-label" for="input-nama-kelas">Nama Kelas</label>
                      <input type="text" id="input-nama-kelas" class="form-control" placeholder="XII" v-model="detailKelas.nama_kelas" disabled>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                          <label class="form-control-label" for="input-username">Kompetensi Kehalian</label>
                          <input type="text" id="input-username" class="form-control" placeholder="Rekayasa Perangkat Lunak" v-model="detailKelas.kompetensi_keahlian" disabled>
                    </div>
                  </div>
                </div>
              <button type="button" class="btn btn-primary" data-dismiss="modal">Tutup</button>
              </form>
            </div>
          </div>
        </div>
      </div>
        <!-- END MODAL -->
                  </tr>
                </tbody>
              </table>
            </div>
            <!-- <pre>{{detailKelas}}</pre> -->
          </div>
          <div class="modal fade" id="modalKelas" tabindex="-1" role="dialog" aria-labelledby="modalEditKelasLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="modalEditKelasLabel"><span v-if="action === 'create'">Tambah</span> <span v-else>Edit</span> Kelas</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form> 
                <div class="col">
                  <div class="form-group">
                    <div class="row">
                      <label class="form-control-label" for="input-nama-kelas">Nama Kelas</label>
                      <input type="text" id="input-nama-kelas" class="form-control" placeholder="XII" v-model="nama_kelas">
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                          <label class="form-control-label" for="input-username">Kompetensi Kehalian</label>
                          <input type="text" id="input-username" class="form-control" placeholder="Rekayasa Perangkat Lunak" v-model="kompetensi_keahlian">
                    </div>
                  </div>
                </div>
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Tutup</button>
              <button type="button" @click="save" :class="buttonClass" data-dismiss="modal"><span v-if="action === 'create'" >Tambah</span> <span v-else>Edit</span>!</button>
              </form>
            </div>
            <!-- <div class="modal-footer">
            </div> -->
          </div>
        </div>
      </div>
      
          </div>
        </div>

      </div>
</template>

<script>
export default {
    data() {
        return {
            alert: false,
            id_kelas: '',
            kelas: '',
            detailKelas: '',
            
            nama_kelas: '',
            kompetensi_keahlian: '',
            
            action: '',
            buttonClass: '',

            // alert
            alertVariant: '',
            alertMessage: ''
        }
    },
    methods: {
        getKelas() {
            this.axios.get('kelas').then(response => {
                this.kelas = response.data.data
            })
        },
        getDetailKelas(index) {
            this.id_kelas = this.kelas[index].id_kelas
            this.axios.get('kelas/'+this.id_kelas).then(response => {
                this.detailKelas = response.data.data
            })
        },
        createKelas() {
          this.action = 'create'
          this.buttonClass = 'btn btn-success'
          this.nama_kelas = ''
          this.kompetensi_keahlian = ''
        },
        updateKelas(index) {
          this.action = 'update'
          this.buttonClass = 'btn btn-primary'
          this.id_kelas = this.kelas[index].id_kelas
          this.nama_kelas = this.kelas[index].nama_kelas
          this.kompetensi_keahlian = this.kelas[index].kompetensi_keahlian
        },
        save() {
          const dataKelas = {
            nama_kelas: this.nama_kelas,
            kompetensi_keahlian: this.kompetensi_keahlian,
          }
          if (this.action === 'create') {
            this.axios.post('kelas', dataKelas).then(response => {
              this.alertSuccess()
              console.log(response);
              this.getKelas()
            }).catch(error => {
              this.alertFailed()
              console.log(error.response)
            })
          } else {
            this.axios.put('kelas/'+this.id_kelas, dataKelas).then(response => {
              this.alertSuccess()
              console.log(response)
              this.getKelas()
            }).catch(error => {
              this.alertFailed()
              console.log(error.response)
            })
          }
        },
        deleteKelas(index) {
          this.action = 'delete'
          this.id_kelas = this.kelas[index].id_kelas
          this.axios.delete('kelas/'+this.id_kelas).then(response => {
            this.alertSuccess()
            console.log(response);
            this.getKelas()
          }).catch(error => {
            this.alertFailed()
            console.log(error.response);
          })
        },
        alertSuccess() {
          this.alert = true
          this.alertVariant = 'alert-success'
          if (this.action === 'create') {
            this.alertMessage = "Berhasil menambahkan data Kelas"
          } else if (this.action === 'update') {
            this.alertMessage = "Berhasil mengubah data Kelas"
          } else {
            this.alertMessage = "Berhasil menghapus data Kelas"
          }
        },
        alertFailed() {
          this.alert = true
          this.alertVariant = 'alert-warning'
          if (this.action === 'update') {
            this.alertMessage = "Gagal menambahkan data Kelas"
          } else if (this.action === 'update') {
            this.alertMessage = "Gagal mengubah data Kelas"
          } else {
            this.alertMessage = "Gagal menghapus data Kelas"
          }
        }
    },
    mounted() {
        this.getKelas()
    }
}
</script>

<style>

</style>