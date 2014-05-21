# İstekler (Requests) ve Girdi (Input)

- [Temel Girdi](#basic-input)
- [Çerezler (Cookies)](#cookies)
- [Önceki Girdi](#old-input)
- [Dosyalar](#files)
- [İstek Bilgileri](#request-information)

<a name="basic-input"></a>
## Temel Girdi

Tüm kullanıcı girdisine birkaç basit metodla erişebilirsiniz. İstek için kullanılmış olan HTTP eylemi için endişe etmenize gerek yoktur, bütün eylemler için girdi bilgisine erişim aynıdır.

#### Bir Girdi Değerinin Öğrenilmesi

	$ismi = Input::get('ismi');

#### Bir Girdi Değerinin (Eksik Olması Durumunda Varsayılacak Olan Bir "Ön Değer" Belirtilerek) Öğrenilmesi

	$ismi = Input::get('ismi', 'Saliha');

#### Bir Girdi Değerinin Mevcut Olduğunun Test Edilmesi

	if (Input::has('ismi'))
	{
		//
	}

#### İstekteki Tüm Girdi Değerlerinin Öğrenilmesi

	$girdi = Input::all();

#### İstek Girdisinin Sadece Bazı Değerlerinin Öğrenilmesi

	$girdi = Input::only('kullaniciadi', 'sifre');

	$girdi = Input::except('kredi_karti');

"Dizi" girdileri olan formlarda çalışırken dizilere erişim için nokta gösterimini kullanabilirsiniz:

	$input = Input::get('products.0.name');

> **Not:** Bazı JavaScript kütüphaneleri, örneğin Backbone, girdi bilgisini uygulamaya JSON olarak gönderir. Bu girdi verisine de yine normal şekilde `Input::get` ile erişebilirsiniz.

<a name="cookies"></a>
## Çerezler (Cookies)

Laravel çerçevesi tarafından oluşturulan tüm çerezler, bir kimlik doğrulama kodu ile şifrelenir ve imzalanır. Kullanıcı tarafından değiştirilmesi halinde geçersiz kabul edilecektir.

#### Bir Çerez Değerinin Öğrenilmesi

	$deger = Cookie::get('ismi');

#### Cevaba (Response) Yeni Bir Çerez İliştirilmesi

	$yanit = Response::make('Merhaba Dünya');

	$yanit->withCookie(Cookie::make('ismi', 'degeri', $dakika));

#### Sonraki Cavap İçin Kuyruğa Bir Çerez İliştirilmesi

Cevap oluşturulmadan önce bir çerez oluşturmak isterseniz, `Cookie::queue()` metodunu kullanın. Uygulamanızdan gelen son cevaba yeni bir çerez iliştirilecektir.

	Cookie::queue($name, $value, $minutes);

#### Süresiz Bir Çerez Oluşturulması

	$cerez = Cookie::forever('ismi', 'degeri');

<a name="old-input"></a>
## Önceki Girdi

Bazı durumlarda bir isteğin girdisini bir sonraki isteğe kadar tutmanız gerekebilir. Örneğin, doğrulama hataları için kontrol ettikten sonra bir formu yeniden bu önceki girdi bilgisi ile doldurmak gerekebilir.

#### Girdinin Oturuma(Session) Geçici Olarak Yansıtılması (flash)

	Input::flash();

#### Girdinin Sadece Bazı Değerlerinin Oturuma Geçici Olarak Yansıtılması

	Input::flashOnly('kullaniciadi', 'email');	//sadece belirtilenler

	Input::flashExcept('sifre');	//belirtilenler hariç

Girdinin geçici olarak oturuma yansıtılmasını, sık şekilde bir önceki sayfaya tekrar-yönlendirme (redirect) ile birlikte yapacağınız için, bu yansıtmayı (redirect)'e zincir ek yapabilirsiniz.

	return Redirect::to('form')->withInput();	//tüm girdi değerleri ile beraber

	return Redirect::to('form')->withInput(Input::except('sifre'));	//belirtilenler hariç

> **Not:** Diğer verilerin istekler arasında geçici yansıtmasını (flash), Oturum [Session](/docs/session) sınıfını kullanarak  yapabilirsiniz.

#### Önceki Girdi Verisinin Elde Edilmesi

	Input::old('kullaniciadi');

<a name="files"></a>
## Dosyalar

#### Yollanan Bir Dosyanın Öğrenilmesi

	$dosya = Input::file('foto');

#### Bir Dosya Yollanmış Olup Olmadığının Belirlenmesi

	if (Input::hasFile('foto'))
	{
		//
	}

Dosya `file` metodu tarafından döndürülen nesne, PHP `SplFileInfo` sınıfının bir uzantısı olan ve dosya ile etkileşim için çeşitli metodlar sağlayan `Symfony\Component\HttpFoundation\File\UploadedFile` sınıfının bir olgusudur.

#### Yüklenmiş Olan Bir Dosyanın Geçerli Olup Olmadığının Belirlenmesi

	if (Input::file('foto')->isValid())
	{
		//
	}

#### Yüklenmiş Olan Bir Dosyanın Taşınması

	Input::file('foto')->move($hedefDizinPatikasi);

	Input::file('foto')->move($hedefDizinPatikasi, $dosyaAdi);

#### Yüklenmiş Olan Bir Dosyanın Dosya Yolunun Öğrenilmesi

	$patika = Input::file('foto')->getRealPath();

#### Yüklenmiş Olan Bir Dosyanın Orijinal Adının Öğrenilmesi

	$name = Input::file('foto')->getClientOriginalName();

#### Yüklenmiş Olan Bir Dosyanın Uzantısının Öğrenilmesi

	$uzanti = Input::file('foto')->getClientOriginalExtension();

#### Yüklenmiş Olan Bir Dosyanın Boyutunun Öğrenilmesi

	$buyukluk = Input::file('foto')->getSize();

#### Yüklenmiş Olan Bir Dosyanın MIME Türünün Öğrenilmesi

	$mime = Input::file('foto')->getMimeType();

<a name="request-information"></a>
## İstek Bilgileri

İstek `Request` sınıfı, uygulamanıza gelecek olan HTTP isteğini incelemeniz için birçok metod sunar ve `Symfony\Component\HttpFoundation\Request` sınıfının bir uzantısıdır. Bunlardan bazıları şöyledir.

#### İstek URI'nın Öğrenilmesi

	$uri = Request::path();

#### Retrieving The Request Method

	$method = Request::method();

	if (Request::isMethod('post'))
	{
		//
	}

#### İstek Patikasının Bir Desene Uygunluğunun Test Edilmesi

	if (Request::is('admin/*'))
	{
		//
	}

#### İstek URL'nin Öğrenilmesi

	$url = Request::url();

#### İstek URI'nın Herhangi Bir Bölümünün Öğrenilmesi

	$segment = Request::segment(1);

#### Bir İstek Başlığı(Header) Değerinin Öğrenilmesi

	$deger = Request::header('Content-Type');

#### Sunucu bilgileri için $_SERVER Değerlerinin Öğrenilmesi

	$deger = Request::server('PATH_INFO');

#### İsteğin HTTPS Üzerinden Olup Olmadığının Belirlenmesi

	if (Request::secure())
	{
		//
	}

#### İsteğin AJAX Kullanıyor Olup Olmadığının Belirlenmesi

	if (Request::ajax())
	{
		//
	}

#### İsteğin JSON Content Tipine Sahip Olup Olmadığının Belirlenmesi

	if (Request::isJson())
	{
		//
	}

#### İsteğin JSON İstiyor Olup Olmadığının Belirlenmesi

	if (Request::wantsJson())
	{
		//
	}

#### İstenen Cevap Biçiminin Kontrol Edilmesi

`Request::format` metodu HTTP Accept header'ine dayalı olarak, istenen cevap formatını döndürecektir:

	if (Request::format() == 'json')
	{
		//
	}