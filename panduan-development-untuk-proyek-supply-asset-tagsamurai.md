# Panduan Development untuk Proyek Supply Asset Tagsamurai

## 1. Alur Pengerjaan

{% stepper %}
{% step %}
### Definisikan Semua Task

* Tentukan cakupan tugas yang akan dikerjakan.
* Pastikan setiap tugas memiliki spesifikasi yang jelas.
{% endstep %}

{% step %}
### Pembuatan API Service

* Buat API Service sesuai dengan spesifikasi yang telah didefinisikan.
* Pengerjaan di branch dev. Tidak perlu membuat branch baru.
* Setelah selesai, langsung push untuk update package version.
{% endstep %}

{% step %}
### Penerapan Test-Driven Development (TDD)

* Pengembangan dilakukan langsung di **Cypress**.
* Kode untuk koneksi API dibuat sejak awal.
* Intercept request menggunakan **dummy fixture** yang sesuai dengan API spesifikasi di **Cypress**.
* Pastikan **PRD dibaca dan dibuat test case yang lengkap** sebelum implementasi kode.
{% endstep %}

{% step %}
### Open pull request dari branch fitur ke branch dev

* Pada proses ini, workflow review akan dijalankan. Workflow review ini memastikan bahwa coverage dari branch sumber sudah mencapai target minimal.
* Branch dev harus berisi kode yang sudah diuji. Karena satu repo modul bisa dikerjakan oleh banyak orang dan branch fitur baru perlu dibuat dari branch dev, ini bertujuan agar pengerjaan fitur lain tidak terhambat karena pengujian pada fitur lainnya belum selesai.
* Ketika membuat pull request, tandai task di Workspace sebagai Selesai untuk melanjutkan ke tahap review leader. Pull request akan digabung ke dev oleh leader setelah lolos review.
{% endstep %}

{% step %}
### Pengujian integrasi di dev console

* Di sini, integrasi antar modul dan ke API diuji.
{% endstep %}
{% endstepper %}

***

## 2. Workflow Baru dalam Development

* Mulai dengan membuat test case di Cypress sebelum implementasi kode.
* Misalnya, saat mengembangkan fitur **slicing table filter**, buat dulu:
  * `describe`, `it`, dan test case untuk mengecek render di Cypress.
* Frontend tidak perlu menunggu Backend:
  * API spesifikasi dibuat sejak awal sehingga FE bisa langsung bekerja.
  * Setelah FE dan BE selesai, integrasi diuji bersama di **console development**.
  * Jika ada perbedaan dengan API spec, dapat langsung diidentifikasi apakah dari FE atau BE.

***

## 3. Best Practice dalam Testing

{% stepper %}
{% step %}
### Mengoptimalkan Tampilan Viewport di Cypress

* Konfigurasi viewport Cypress diatur ke 1280×720 sesuai ukuran desain Figma.
*   Untuk memaksimalkan tampilan, lakukan beberapa trik berikut:

    * Jika perlu menggunakan devtools, lepaskan sebagai jendela terpisah.

    ![](<.gitbook/assets/5670ff97 8906 4b10 88bc cbcb4089fc43.png>)

    * Optimalkan viewport agar satu halaman penuh dengan mengurangi level zoom halaman dan menyesuaikan ukuran sidebar Specs di sebelah kiri. Ini mungkin tergantung pada ukuran layar. Dalam contoh di bawah, level zoom 80% adalah yang paling cocok.

    ![](<.gitbook/assets/44f24d8b a857 4156 8b05 7e61b023626a.png>)

    * Zoom level tidak akan mempengaruhi tampilan render Cypress, karena berada di dalam iframe.
    * Jika perlu fokus pada elemen yang cukup kecil di halaman, gunakan touchpad dengan dua jari untuk memperbesar layar.

    ![](<.gitbook/assets/1324e3f2 c36a 4efd b81a 24ed87998385.png>)
{% endstep %}

{% step %}
### Testing untuk Request API

* Pastikan setiap request **POST/PUT/DELETE** dicek **body payload-nya**.
* Termasuk dalam bentuk **formData** jika ada. Gunakan plugin [cypress-intercept-form-data](https://www.npmjs.com/package/cypress-intercept-formdata#cypress-intercept-formdata-cifd)
* Ini untuk mencegah **regression bug** pada pengembangan berikutnya.
{% endstep %}

{% step %}
### Coverage Testing

* Targetkan **coverage minimal 96%**.
* Dapat dicek di local dengan `pnpm check-coverage`.
* **SonarQube tidak digunakan lagi**, coverage sudah dipastikan tetap sama saat direview di CI/CD.
{% endstep %}

{% step %}
### Test harus mencakup seluruh user flow

* Tidak hanya fungsi yang kita buat, tetapi juga **user flow di PRD**.
{% endstep %}
{% endstepper %}

***

## 4. Implementasi dan Coding Standard

Berisi hal-hal yang boleh dan tidak boleh dilakukan berdasarkan hasil review.

{% stepper %}
{% step %}
### Pengecekan Toast message

* Sebaiknya gunakan `cy.contains("message")` untuk validasi pesan toast, bukan `cy.get('[data-pc-name="toast"]')`.
{% endstep %}

{% step %}
### Setup Loading Indicator pada Aksi PUT/POST/DELETE/PATCH

Contoh:

```typescript
try {
       setLoading(true, 'Processing your request, please wait...');
} finally {
       setLoading(false);
}
```
{% endstep %}

{% step %}
### Menetapkan Initial Value pada Form Dialog

* Gunakan metode `setValues` pada `DialogForm` untuk mengatur data awal.
* Tidak perlu `:initial-value` prop di masing-masing input field.
* Hanya gunakan `v-model` jika perlu untuk conditional rendering, bisa juga alternatif `#fields="{ formValues }"` slotProps dari Form/DialogForm Component.
* Berlaku juga untuk `Form` component.

Contoh implementasi:

```typescript
const setInitialGroupData = async (): Promise<void> => {
  if (props.action === 'Create') return;
  if (props.group === undefined) return;

  try {
    const { data } = await GroupService.getDetailAndSubs(props.group?._id, {
      withoutData: 'true',
    });

    dialogForm.value?.setValues({
      name: data.data.detail.name,
      aliasCode: data.data.detail.aliasCode,
      items: data.data.detail.items,
      image: data.data.detail.image,
      isAdministrativeGroup: data.data.detail.isAdministrativeGroup,
      isDisposalGroup: data.data.detail.isDisposalGroup,
      tagType: data.data.detail.tagType,
    });

    initialGroupName.value = data.data.detail.name;
    initialAliasCode.value = data.data.detail.aliasCode;
  } catch (error) {
    console.error(error);
  }
};
```
{% endstep %}

{% step %}
### Menggunakan Config Warna yang Sudah Ada

* Jangan gunakan warna hardcoded seperti `text-[#785930]`. Gunakan konfigurasi palet warna yang sudah tersedia di `colors-config.json`. Cari nama palet yang sesuai dengan kode warna hex yang dibutuhkan.
{% endstep %}

{% step %}
### Validasi Query Parameter dalam Cypress

Contoh:

```javascript
cy.wait('@getDetail').its('request.query.search').should('eq', undefined);
cy.get('@getDetail')
     .its('request.query.isAdministrativeGroup')
     .should('eq', JSON.stringify([true, false]));
cy.get('@getDetail')
     .its('request.query.isDisposalGroup')
     .should('eq', JSON.stringify([true, false]));
cy.get('@getDetail')
     .its('request.query.tagType')
     .should('eq', JSON.stringify(['Non TAG']));
```
{% endstep %}
{% endstepper %}

***

## 5. Version Control & Deployment

* Gunakan branch feature dan buat PR ke branch `dev`.
* Branch fitur dapat dibuat untuk setiap tugas yang ditentukan di workspace.
* Frontend dapat melakukan deploy tanpa menunggu Backend, nanti akan dilakukan **test integrasi di console dev**.
* Jika ada kendala dalam workflow ini, bisa dibahas bersama untuk perbaikan.

***

## Kesimpulan

Panduan ini dirancang untuk memastikan pengembangan proyek Supply berjalan efisien dengan pendekatan **Test-Driven Development (TDD)** berbasis Cypress. Dengan mengikuti metode ini, tim frontend dapat bekerja secara paralel dengan backend tanpa menunggu implementasi selesai, sehingga mempercepat pengembangan dan mengurangi potensi bug dalam sistem.

Updated on December 8, 2025, 8:34 AM UTC

* Standardized Cypress E2E and Unit Testing: https://fewangsit.hashnode.space/docs/cypress-e2e-and-unit-testing-guidelines

![](<.gitbook/assets/5670ff97 8906 4b10 88bc cbcb4089fc43.png>)

![](<.gitbook/assets/44f24d8b a857 4156 8b05 7e61b023626a.png>)

![](<.gitbook/assets/1324e3f2 c36a 4efd b81a 24ed87998385.png>)
