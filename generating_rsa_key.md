# Generating RSA key

Using [OpenSSL](https://www.openssl.org/) command to generate RSA key pair.

**Generate a 2048 bit RSA private key**

```bash
openssl genrsa -out private.pem 2048
```

The content of **private.pem** as follows.

> -----BEGIN RSA PRIVATE KEY-----
> MIIEpAIBAAKCAQEA2fwUOF0oksT44D2JTtJHKceytQMyCS3Nxj/rzZ2ZlF1wjACE
> 7b5EWDtjtHQrf7MtD0KRk3omc9QXZO36DekvqcV5CrCQ6+W5ofEcSo7fjaUzuM3t
> 4YTwxBxM8nZr/dns5y6mE+BZtR9puicmD4x/jwO71wTLtPC/UFnZYtqt64rPMqZu
> ms25HPeagM1pD9ygMAkRRnpE6ouWyafX+W2RQGu5UY/Ai6HqYap0WjFRcQXHC78/
> Mpx2G9CpqGZxRmeCzfE8XKY7ETM2NBXwaesBslTQGnVrN0U2rDsiYrBUyBjbgrS7
> 8YX3sgp+B6SPvMYA1kJytWgfM10sqVXhznyOMwIDAQABAoIBAQCjeQPA8TwINWde
> 40chsVhk4LoIPYk8kPkMf8tau6H9PcW1eH43vMSMVp2DKsukTL6x/v4lVymXG6sf
> qcgovoNcEhegCKHmBrWb2LGayGKmWwnawbRvt77Hk2GxQ1XhXZjoFne92DXuOsyT
> KeDFMFxI6EfqDFKoMxOGMptwIwbi6CgAJGMiz5x2qHyl/Ik+fEXRD1PSyQ6zLMiz
> IB6OwmV8lKGud8ZKH762b1KBqhw0t4urKqlpAQUsjmi/Q1FCP4E6qRE3yWflXL+D
> 8OlTu6xpjjFteeuzMvVzWhU8ZZs6n3gc+mLxfqmL1QeFLwBXWdeiBmPolwoQhU/x
> 8Gz2IqyBAoGBAO3INxkmg512OYRrlds6XzmwYAVwZLQLR76wStenAxyI7PKNOMrO
> V3eOomXzT9heY/l9iWkMjMnuLUwRyBaTZ2DxhNbwQPSB7D0jPpY0TyMuOzO4MOGQ
> DKSdG5t9n4jkAdObWB457o/7+SviZrcgA04ZMC5Ijlf0Y7z7C13bFWFxAoGBAOqv
> kEHE8srMsvFxJ+Pd6425Xg5t/OO2A6xDPMgSvTDHcZ4ksbFJVVHvcJn9znYuzWNI
> YUZI19LHAch78wvEmiXESNTYPrDYd6OTeYE65U/aJ+4Jvjqev4QBFTCP9eZ/AVPN
> rhTaO1G8810NL1DxduKDk7SLNnr4o3fvdguoJhfjAoGBAJ/ErpjckwTDQkRikY97
> Oi6l/u7IpTGAftV22OLr2iBbNHKJR0alvImdsiq0gMrOKXiizChkgVjRC2iYbgwV
> QRoXTf2p8ssXXSd7Pfto7F+kGa1XrqhwxL36vmkM0JwHL98B+wRcQppGYRcGiaO0
> A+R+8iu3HsWkdTuupuWKZmRRAoGAetzqwuPez/kWfXxmC890sD+pVBiU2onBpn+U
> 5JGa5lyjyM0hEdV4i2q1IolTe1/JOv77nhYolzqEXnc1qKWGpdr63iNPvrm0+LgO
> Vm+E+acWXHJRWtMdJHiEpWXYsJExGrSFPHl7sLEhH0f1y4R+XtvPiiePoBVnTzTY
> MgYX200CgYBN9GP8/g4FvfluMPqBEdFSYqv1rkUThkXTOiSv1hdbpmxdViFLJae6
> LoGV3vueQG5ujXLa18rh9givMuxdfxmUYynJ/eGGCmN0ogfpqsZ9Ai7uiza7yMfA
> F1gyrI8gJsi07sQSgbVLMhKwrLUdm81fSD+K8FCYQgRjwy67ouQ0dw==
> -----END RSA PRIVATE KEY-----

**Generate the RSA public key from the private key**

```bash
openssl rsa -in private.pem -outform PEM -pubout -out public.pem
```

The content of **public.pem** as follows.

> -----BEGIN PUBLIC KEY-----
> MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2fwUOF0oksT44D2JTtJH
> KceytQMyCS3Nxj/rzZ2ZlF1wjACE7b5EWDtjtHQrf7MtD0KRk3omc9QXZO36Dekv
> qcV5CrCQ6+W5ofEcSo7fjaUzuM3t4YTwxBxM8nZr/dns5y6mE+BZtR9puicmD4x/
> jwO71wTLtPC/UFnZYtqt64rPMqZums25HPeagM1pD9ygMAkRRnpE6ouWyafX+W2R
> QGu5UY/Ai6HqYap0WjFRcQXHC78/Mpx2G9CpqGZxRmeCzfE8XKY7ETM2NBXwaesB
> slTQGnVrN0U2rDsiYrBUyBjbgrS78YX3sgp+B6SPvMYA1kJytWgfM10sqVXhznyO
> MwIDAQAB
> -----END PUBLIC KEY-----
