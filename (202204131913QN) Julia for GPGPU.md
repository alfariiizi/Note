---
category: "Quick Notes"
real-name: "Julia for GPGPU"
---

- field-related:: [[Dev]]
- creation-date:: [[2022-04-13]]
- tags:: #notes/quick

---

# Julia for GPGPU

## Notes

### Julia-GPU

> Src: <https://www.notamonadtutorial.com/julia-gpu/>

- **Tujuan dari penulisan JuliaGPU** adalah:
	- Memudahkan pengguna GPU untuk semua user.
	- Memberikan performance yang bagus, meskipun Julia dapat diakses secara high-level.
	- Sehingga, tujuan dari JuliaGPU itu sendiri adalah: Performance ☑️, Easy to use ☑️.
- **Apakah Julia cocok digunakan sebagai bahasa untuk penggunaan GPGPU?**
	- Julia mempunyai compiler nya sendiri. Dan compiler nya pun akan menghasilkan machine code yang compact dan mempunyai performa tinggi.
	- Julia dapat secara langsung meng-compile code nya ke GPU. Sehingga kita bisa membuat suatu function/kernel tersendiri, kemudian meng-compile nya ke GPU.
		- > Mungkin yang dimaksud disini adalah: Kita tidak perlu bahasa penyalur code lain (seperti C/C++/Rust) ke GPU seperti GLSL maupun HLSL. Kita bisa langsung menuliskan code nya di Julia, kemudian Julia akan meng-compile nya ke GPU.
- **Bagaimana performanya jika dibandingkan dengan menggunakan bahasa Python?**
	- Python mempunyai module _PyCUDA_. Julia mempunyai module bernama _CUDA.jl_.
	- Dalam segi penulisan, Python dan Julia memiliki banyak kemiripan, yakni simplicity. Cukup beberapa baris code saja bisa menghasilkan program yang kita inginkan.
	- Dalam segi performa, Julia jauh lebih baik daripada Python. Hal ini dikarenakan Julia di-compile menggunakan compiler nya sendiri. Sedangkan Python dituliskan diatas C, sehingga pada dasarnya perlu waktu tambahan (dalam mengeksekusi program) untuk dapat menjalankan code Python.
- **Kelebihan - kelebihan lainnya pada Julia** adalah:
	- Kita dapat mengimplementasikan kernel/function yang sesuai dengan kebutuhan kita. Namun Julia juga telah mempunyai banyak default operation, sehingga kita tidak perlu menuliskan semua operasi yang umum dilakukan.
	- Pada CUDA.jl, hampir semua object nya telah terintegrasi dengan code murni dari Julia.
		- Hal tersebut tentunya sangat berbeda dari C/C++ yang mana membutuhkan GLSL untuk dituliskan sebagai shader nya, kemudian diperlukan library GLM agar bisa menggunakan data type (object) yang ada pada GLSL.
		- Pada Julia, Integrasi code nya tersebut cukup mirip seperti yang ada di Python. Misalnya saja pada kebanyakan pandas akan support array bertipe `ndarray`, `list`, `tuple`, dll.
	- Terdapat pula _bound checker_ yang mana dapat mendeteksi error ketika mengeksekusi kernel-kernel yang ada pada Julia (CUDA.jl).
		- Bound checker ini tentunya akan memperlambat performa yang ada. Untuk itu, jika sudah yakin dengan code yang dijalankan, maka matikan saja bound checker nya ini dengan cara menuliskan `@inbounds`.

### Reddit

- <https://www.reddit.com/r/MachineLearning/comments/791ssf/news_highperformance_gpu_computing_in_the_julia/>
	- u/Skariel:
		- Julia sangat memudahkan dalam pengimplementasian GPGPU, namun disisi lain Julia juga mempunyai performa program yang sangat baik.
		- he main advantage of languages like Python and Julia is interactivity. Julia gives you both interactivity and speed. Python lacks in speed and C++ lacks interactivity
		- Salah satu kelebihan utama menggunakan Julia adalah: _Interactivity_ + _Performance_.
			- Python mempunyai kelebihan dalam segi interactivity, namun sangat kurang di performance.
			- C/C++ mempunyai kelebihan dalam performance, namun cukup kurang di interactivity nya.
	- u/Outlacedev:
		- It has macros and really powerful introspection tools that make it possible to basically write "raw" arbitrary Julia code, get the gradients, run on GPU, etc (but you don't need to know that stuff, it just makes it great for developers to make awesome packages). Being able to just write Julia rather than use a domain-specific language (which is basically what TensorFlow and PyTorch are) is really freeing.
		- If some functionality is missing in a package, you can implement it yourself since it's all Julia (vs Python where all the fast libraries use C or Cython); that's similar to what this post is showing, if you want to implement some GPU kernel that doesn't exist, you can do it in Julia rather than have to learn/brush up on C/C++.
		- Plus, just syntactically working with native multi-dimensional arrays in Julia is just nicer than numpy arrays.
- <https://www.reddit.com/r/programming/comments/jez0ut/julia_gpu_how_the_julia_language_is_making_it/>
	- Overall, cukup banyak yang menerima manfaat dari menggunakan Julia (CUDA.jl), instead of C ataupun Python.


## Related

```dataview
LIST 
FROM [[(202204131913QN) Julia for GPGPU]] OR outgoing([[(202204131913QN) Julia for GPGPU]]) AND -"Excalidraw"
WHERE file.link != this.file.link
SORT file.name ASC
```
