<template>
  <div class="row">
        <div class="col">
          <div class="card">
            <div v-if="siswa.length == 0">
              <div class="card-header">
                <div class="col" style="text-align:center;  margin-bottom:50px; ">
                  <!-- <div style="height: 100%; display:block; padding:auto 0"> -->
                    <img src="../../public/assets/img/theme/vue.jpg" alt="" srcset="">
                    <p>Ups. Belum ada data siswa nih..</p>
                    <button class="btn btn-success" @click="createSiswa" data-toggle="modal" data-target="#modalSiswa"><span class="alert-icon"><i class="ni ni-fat-add"></i></span>Siswa</button>
                  <!-- </div> -->
                </div>
              </div>
            </div>
            <div v-else>
            <div v-if="siswa == ''">
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
              <div class="alert alert-dismissible fade show" :class="alertVariant" role="alert" v-if="alert">
                <!-- <span class="alert-icon"><i class="ni ni-like-2"></i></span> -->
                <span class="alert-text"><strong>{{alertMessage}}</strong></span>
                <button type="button" class="close" data-dismiss="alert" aria-label="Close" @click="!alert">
                    <span aria-hidden="true">&times;</span>
                </button>
              </div>
              <button class="btn btn-success" @click="createSiswa" data-toggle="modal" data-target="#modalSiswa"><span class="alert-icon"><i class="ni ni-fat-add"></i></span>Siswa</button>
              
            </div>
            <!-- Light table -->
            <div class="table-responsive">
              <table class="table align-items-center table-flush">
                <thead class="thead-light">
                  <tr>
                    <th></th>
                    <th scope="col">No.</th>
                    <th scope="col">Nama Siswa</th>
                    <th scope="col">Kelas</th>
                    <th scope="col">Alamat</th>
                    <th scope="col">No. Telepon</th>
                    <th scope="col">Aksi</th>
                  </tr>
                </thead>
                <tbody class="list">
                  <tr v-for="(siswa, index) in siswa" :key="index">
                    <td></td>
                    <td>
                        {{index+1}}
                    </td>
                    <td>
                      {{siswa.nama}}
                    </td>
                    <td>
                      <span v-if="!siswa.kelas">-</span>
                      <span v-else>{{siswa.kelas.nama_kelas}}</span>
                    </td>
                    <td>
                      {{siswa.alamat}}
                    </td>
                    <td>
                      {{siswa.no_telp}}
                    </td>
                    <!-- <td>
                      <span class="badge badge-dot mr-4">
                        <i class="bg-warning" v-if="siswa.level == 'admin'"></i>
                        <i class="bg-success" v-else></i>
                        {{siswa.level}}
                      </span>
                    </td> -->
                    <td>
                        <button class="btn btn-success" data-toggle="modal" :data-target="'#modalDetailSiswa'+index" @click="getDetailSiswa(index)">Detail</button>
                        <button class="btn btn-primary" @click="updateSiswa(index)" data-toggle="modal" data-target="#modalSiswa" >Edit</button>
                        <button class="btn btn-danger" @click="deleteSiswa(index)">Delete</button>
                    </td>

                    <!-- MODAL DETAIL -->
                    <div class="modal fade" :id="'modalDetailSiswa'+index" tabindex="-1" role="dialog" aria-labelledby="modalDetail" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="modalDetail">Detail Siswa</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form> 
                <div class="col">
                  <div class="form-group">
                    <div class="row">
                      <label class="form-control-label" for="input-nis">NIS</label>
                      <input type="number" id="input-nis" class="form-control" placeholder="Nomor Induk Siswa" v-model="detailSiswa.nis" disabled>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                      <label class="form-control-label" for="input-name">Nama Siswa</label>
                      <input type="text" id="input-name" class="form-control" placeholder="Nama lengkap siswa" v-model="detailSiswa.nama" disabled>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                          <label class="form-control-label" for="input-kelas">Kelas</label>
                          <select v-model="detailSiswa.id_kelas" class="form-control" id="input-kelas" disabled>
                              <option value="" disabled selected>Pilih Kelas</option>
                              <option v-for="(kelas, index) in kelas" :key="index" :value="kelas.id_kelas">{{kelas.nama_kelas}} - {{kelas.kompetensi_keahlian}}</option>
                          </select>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                          <label class="form-control-label" for="input-no_telp">Nomor Telepon</label>
                          <input type="tel" id="input-no_telp" class="form-control" placeholder="Nomor telepon siswa" v-model="detailSiswa.no_telp" disabled>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                          <label class="form-control-label" for="input-alamat">Alamat</label>
                          <input type="text" id="input-alamat" class="form-control" placeholder="Alamat" v-model="detailSiswa.alamat" disabled>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                      <label class="form-control-label" for="input-spp">SPP</label>
                      <select id="input-spp" class="form-control" v-model="detailSiswa.id_spp" disabled>
                        <option value="" disabled selected>Pilih SPP</option>
                        <option v-for="(spp, index) in spp" :key="index" :value="spp.id_spp">Tahun {{spp.tahun}} - {{spp.nominal}}</option>
                      </select>
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

            </div>
            <!-- <pre>{{siswa}}</pre> -->
            <!-- MODAL SISWA -->
            <div class="modal fade" id="modalSiswa" tabindex="-1" role="dialog" aria-labelledby="modalEditSiswaLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="modalEditSiswaLabel"><span v-if="action === 'create'">Tambah</span> <span v-else>Edit</span> Siswa</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form> 
                <div class="col">
                  <div class="form-group">
                    <div class="row">
                      <label class="form-control-label" for="input-nis">NIS</label>
                      <input type="number" id="input-nis" class="form-control" placeholder="Nomor Induk Siswa" v-model="nis">
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                      <label class="form-control-label" for="input-name">Nama Siswa</label>
                      <input type="text" id="input-name" class="form-control" placeholder="Nama lengkap siswa" v-model="nama_siswa">
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                          <label class="form-control-label" for="input-kelas">Kelas</label>
                          <select v-model="id_kelas" class="form-control" id="input-kelas">
                              <option value="" disabled selected>Pilih Kelas</option>
                              <option v-for="(kelas, index) in kelas" :key="index" :value="kelas.id_kelas">{{kelas.nama_kelas}} - {{kelas.kompetensi_keahlian}}</option>
                          </select>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                          <label class="form-control-label" for="input-no_telp">Nomor Telepon</label>
                          <input type="tel" id="input-no_telp" class="form-control" placeholder="Nomor telepon siswa" v-model="no_telp">
                    </div>
                  </div>
                  <div class="form-group" v-if="action === 'create'">
                    <div class="row">
                          <label class="form-control-label" for="input-no_telp">Password</label>
                          <input type="tel" id="input-no_telp" class="form-control" placeholder="Password siswa" v-model="password">
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                          <label class="form-control-label" for="input-alamat">Alamat</label>
                          <input type="text" id="input-alamat" class="form-control" placeholder="Alamat" v-model="alamat">
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="row">
                      <label class="form-control-label" for="input-spp">SPP</label>
                      <select id="input-spp" class="form-control" v-model="id_spp">
                        <option value="" disabled selected>Pilih SPP</option>
                        <option v-for="(spp, index) in spp" :key="index" :value="spp.id_spp">Tahun {{spp.tahun}} - {{spp.nominal}}</option>
                      </select>
                    </div>
                  </div>
                </div>
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Tutup</button>
              <button type="button" @click="save" :class="buttonClass" data-dismiss="modal"><span v-if="action === 'create'" >Tambah</span> <span v-else>Edit</span>!</button>
              </form>
            </div>
          </div>
        </div>
      </div>

          </div>
        </div>

      </div>
</template>

<script>
import { ContentLoader } from 'vue-content-loader'

export default {
  components: {
    ContentLoader
  },
    data() {
        return {
            alert: false,
            nisn: '', //as id_siswa
            siswa: '',
            detailSiswa: '',
            
            kelas: '',
            id_kelas: '',
            nis: '',
            nama_siswa:'',
            spp: '',
            nama_kelas: '',
            password: '',
            no_telp: '',
            alamat: '',
            id_spp: '',

            action: '',
            buttonClass: '',

            // alert
            alertVariant: '',
            alertMessage: ''
        }
    },
    methods: {
        getSiswa() {
            this.axios.get('siswa').then(response => {
                this.siswa = response.data.data
            }).catch(error => {
                console.log(error.response);
            })
        },
        getKelas() {
            this.axios.get('kelas').then(response => {
                this.kelas = response.data.data
            }).catch(error => {
                console.log(error.response);
            })
        },
        getSPP() {
            this.axios.get('spp').then(response => {
                this.spp = response.data
            }).catch(error => {
                console.log(error.response);
            })
        },
        getDetailSiswa(index) {
            const nisn = this.siswa[index].nisn
            this.axios.get('siswa/'+nisn).then(response => {
                this.detailSiswa = response.data
            })
        },
        createSiswa() {
          this.action = 'create'
          this.buttonClass = 'btn btn-success'
          this.nis = ''
          this.nama_siswa = ''
          this.alamat = ''
          this.password = ''
          this.id_kelas = ''
          this.no_telp = ''
          this.id_spp = ''
        },
        updateSiswa(index) {
          this.action = 'update'
          this.buttonClass = 'btn btn-primary'
          this.nisn = this.siswa[index].nisn
          this.nis = this.siswa[index].nis
          this.nama_siswa = this.siswa[index].nama
          this.no_telp = this.siswa[index].no_telp
          this.alamat = this.siswa[index].alamat
          this.password = this.siswa[index].password
          this.id_kelas = this.siswa[index].id_kelas
          this.id_spp = this.siswa[index].id_spp
        },
        save() {
          const dataSiswa = {
            nis: this.nis,
            nama: this.nama_siswa,
            no_telp: this.no_telp,
            alamat: this.alamat,
            id_kelas: this.id_kelas,
            id_spp: this.id_spp,
            password: this.password
          }
          if (this.action === 'create') {
            this.axios.post('siswa', dataSiswa).then(response => {
              this.alertSuccess()
              console.log(response);
              this.getSiswa()
            }).catch(error => {
              this.alertFailed()
              console.log(error.response)
            })
          } else {
            this.axios.put('siswa/'+this.nisn, dataSiswa).then(response => {
              this.alertSuccess()
              console.log(response)
              this.getSiswa()
            }).catch(error => {
              this.alertFailed()
              console.log(error.response)
            })
          }
        },
        deleteSiswa(index) {
          this.action = 'delete'
          this.nisn = this.siswa[index].nisn
          this.axios.delete('siswa/'+this.nisn).then(response => {
            this.alertSuccess()
            console.log(response);
            this.getSiswa()
          }).catch(error => {
            this.alertFailed()
            console.log(error.response);
          })
        },
        alertSuccess() {
          console.log('alert success');
          this.alert = true
          this.alertVariant = 'alert-success'
          if (this.action === 'create') {
            this.alertMessage = "Berhasil menambahkan data Siswa"
          } else if (this.action === 'update') {
            this.alertMessage = "Berhasil mengubah data Siswa"
          } else if (this.action === 'delete') {
            this.alertMessage = "Berhasil menghapus data Siswa"
          }
        },
        alertFailed() {
          console.log('alert failed');
          this.alert = true
          this.alertVariant = 'alert-warning'
          if (this.action === 'update') {
            this.alertMessage = "Gagal menambahkan data Siswa"
          } else if (this.action === 'update') {
            this.alertMessage = "Gagal mengubah data Siswa"
          } else if (this.action === 'delete') {
            this.alertMessage = "Gagal menghapus data Siswa"
          }
        }
    },
    mounted() {
        this.getSiswa()
        this.getKelas()
        this.getSPP()
    }
}
</script>

<style>

</style>