<template>
  <ValidationObserver ref="validator" tag="fieldset">
    <div class="form-input-container">
      <FormInput  type="text"
                  name="complete_name"
                  title="Nama Lengkap"
                  placeholder="Masukkan nama lengkap"
                  rules="required"
                  :custom-messages="{required: 'Nama harus diisi'}"
                  v-model="mData.personal.name"/>
    </div>
    <div class="form-input-container">
      <FormInput  type="text"
                  name="birth_city"
                  title="Tempat Lahir"
                  placeholder="Masukkan kota tempat kelahiran"
                  rules="required"
                  :custom-messages="{required: 'Kota kelahiran harus diisi'}"
                  v-model="mData.personal.birth_city" />
    </div>
    <div class="form-input-container">
      <FormInput  type="date"
                  name="birth_date"
                  title="Tanggal Lahir"
                  placeholder="Masukkan tanggal lahir"
                  rules="required"
                  :custom-messages="{required: 'Tanggal lahir harus diisi'}"
                  v-model="mData.personal.birth_date" />
    </div>
    <div class="form-input-container">
      <FormSelect name="religion"
                  title="Agama"
                  :options="choices.religions"
                  rules="required"
                  prompt="Pilih salah satu opsi di bawah ini"
                  :custom-messages="{required: 'Agama harus diisi'}"
                  v-model="mData.personal.religion" />
    </div>
    <div class="form-input-container">
      <FormRadioGroup name="marital_status"
                      title="Status Pernikahan"
                      :options="choices.maritalStatus"
                      rules="required"
                      :custom-messages="{required: 'Status pernikahan harus diisi'}"
                      :value="mData.personal.marital_status || choices.maritalStatus[0]"
                      @change="$set(mData.personal, 'marital_status', $event)" />
    </div>
    <hr class="-mx-8 mb-8">
    <div class="form-input-container">
      <FormTextarea name="ktp_address"
                    title="Alamat sesuai KTP"
                    placeholder="Masukkan alamat yang tertera di KTP"
                    rules="required"
                    rows="4"
                    :custom-messages="{required: 'Alamat harus diisi'}"
                    v-model="mData.personal.ktp_address" />
    </div>
    <div class="form-input-container">
      <FormTextarea name="current_addresss"
                    title="Alamat saat ini"
                    placeholder="Masukkan alamat saat ini"
                    rules="required"
                    rows="4"
                    :custom-messages="{required: 'Alamat harus diisi'}"
                    v-model="mData.personal.current_address">
        <template #title>
          <div class="mb-4 flex flex-row justify-between items-center">
            <label class="form-input__title is-required">
              Alamat Saat Ini
            </label>
            <button class="button border border-solid border-blue-500 text-brand-blue hover:opacity-50 hover:bg-blue-100"
                    @click="$set(mData.personal, 'current_address', mData.personal.ktp_address)">
              IDEM
            </button>
          </div>
        </template>
      </FormTextarea>
    </div>
    <div class="form-input-container">
      <FormInput  type="text"
                  name="phone_number"
                  title="Nomor Telepon (seluler)"
                  placeholder="Masukkan nomor telepon seluler"
                  rules="required|min:10|max:20"
                  :custom-messages="{
                    required: 'Nomor telepon harus diisi',
                    min: 'Nomor telepon minimal terdiri dari 10 angka',
                    max: 'Nomor telepon maksimal terdiri dari 20 angka'
                  }"
                  v-model="mData.personal.phone_number" />
    </div>
    <div class="form-input-container">
      <FormInput  type="email"
                  name="email"
                  title="Email"
                  placeholder="Masukkan alamat email"
                  rules="required|email"
                  :custom-messages="{
                    required: 'Alamat email harus diisi',
                    email: 'Alamat email tidak valid'
                  }"
                  v-model="mData.personal.email" />
    </div>
    <hr class="-mx-8 mb-8">
    <div class="form-input-container">
      <FormInput  type="text"
                  name="ktp_number"
                  title="Nomor KTP"
                  placeholder="Masukkan nomor KTP"
                  rules="required|numeric|length:16|ktp"
                  :custom-messages="{
                    required: 'Nomor KTP harus diisi',
                    numeric: 'Nomor KTP hanya terdiri dari angka',
                    length: 'Nomor KTP terdiri dari 16 angka',
                    ktp: 'Nomor KTP tidak valid'
                  }"
                  v-model="mData.docs.ktp.number" />
    </div>
    <div class="form-input-container">
      <FormInputFile  name="document_ktp"
                      title="Upload KTP"
                      subtitle="File dalam format JPEG/PNG/PDF"
                      rules="required|mimes:image/*,.pdf|size:2048"
                      :custom-messages="{
                        required: 'File KTP belum dipilih',
                        mimes: 'File harus dalam format JPEG/PNG/PDF',
                        size: 'File tidak boleh berukuran lebih dari 2MB'
                      }"
                      accept="image/*,.pdf"
                      :value.sync="mData.docs.ktp.document_url"
                      :filename.sync="mData.docs.ktp.document_name"
                      :file.sync="mData.docs.ktp.document_blob" />
    </div>
    <div class="form-input-container">
      <FormInputFile  name="document_kk"
                      title="Upload KK"
                      subtitle="File dalam format JPEG/PNG/PDF"
                      rules="required|mimes:image/*,.pdf|size:2048"
                      :custom-messages="{
                        required: 'File KK belum dipilih',
                        mimes: 'File harus dalam format JPEG/PNG/PDF',
                        size: 'File tidak boleh berukuran lebih dari 2MB'
                      }"
                      accept="image/*,.pdf"
                      :value.sync="mData.docs.kartu_keluarga.document_url"
                      :filename.sync="mData.docs.kartu_keluarga.document_name"
                      :file.sync="mData.docs.kartu_keluarga.document_blob" />
    </div>
    <div class="form-input-container">
      <FormInput  type="text"
                  name="npwp_number"
                  title="Nomor NPWP"
                  placeholder="Masukkan nomor NPWP"
                  rules="required|numeric|length:15"
                  :custom-messages="{
                    required: 'Nomor NPWP harus diisi',
                    numeric: 'Nomor NPWP hanya terdiri dari angka',
                    length: 'Nomor NPWP terdiri dari 15 angka'
                  }"
                  v-model="mData.docs.npwp.number" />
    </div>
    <div class="form-input-container">
      <FormInputFile  name="document_npwp"
                      title="Upload NPWP"
                      subtitle="File dalam format JPEG/PNG/PDF"
                      rules="required|mimes:image/*,.pdf|size:2048"
                      :custom-messages="{
                        required: 'File NPWP belum dipilih',
                        mimes: 'File harus dalam format JPEG/PNG/PDF',
                        size: 'File tidak boleh berukuran lebih dari 2MB'
                      }"
                      accept="image/*,.pdf"
                      :value.sync="mData.docs.npwp.document_url"
                      :filename.sync="mData.docs.npwp.document_name"
                      :file.sync="mData.docs.npwp.document_blob" />
    </div>
    <hr class="-mx-8 mb-8">
    <div class="form-input-container">
      <FormSelect name="job"
                  title="Posisi saat ini"
                  :options="choices.jobs"
                  rules="required"
                  prompt="Pilih salah satu opsi di bawah ini"
                  :custom-messages="{required: 'Posisi harus diisi'}"
                  v-model="mData.assignment.job" />
    </div>
    <div class="form-input-container">
      <FormInput  type="date"
                  name="working_start_date"
                  title="Tanggal Mulai Kerja"
                  placeholder="Masukkan tanggal mulai kerja"
                  rules="required"
                  :custom-messages="{required: 'Tanggal mulai kerja harus diisi'}"
                  v-model="mData.assignment.start_date" />
    </div>
  </ValidationObserver>
</template>

<script>
export default {
  name: 'EditPersonalData',
  components: {
    FormInput: () => import('@/components/Form/Input'),
    FormInputFile: () => import('@/components/Form/InputFile'),
    FormTextarea: () => import('@/components/Form/Textarea'),
    FormRadioGroup: () => import('@/components/Form/RadioGroup'),
    FormSelect: () => import('@/components/Form/Select')
  },
  props: {
    data: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      choices: {
        maritalStatus: [
          'Sudah Menikah',
          'Belum Menikah'
        ],
        religions: [
          'Islam',
          'Kristen',
          'Buddha',
          'Hindu',
          'Khonghucu'
        ],
        jobs: []
      }
    }
  },
  created () {
    this.$store.dispatch('organizations/fetchJobs')
      .then(jobs => {
        this.$set(this.choices, 'jobs', JSON.parse(JSON.stringify(jobs)))
      })
  },
  computed: {
    mData: {
      get () {
        return this.data
      },
      set (obj) {
        this.$emit('change:data', obj)
      }
    }
  }
}
</script>

<style scoped lang="scss">
hr {
  @apply border-gray-500;
}
</style>
