Proiect Cloud - Achizitii Cereale
Dorobantu Andrei-Denis
Grupa: 1119, Simpre

Link catre videoclip:https://youtu.be/ychbDvUvcTg

Link publicare:https://proiectcloud-3d8e9.firebaseapp.com/#/

Descriere problemă:

Aplicația este o platformă de gestionare a tranzacțiilor între clienți și furnizori în domeniul agricol. Scopul său principal este de a facilita procesul de înregistrare a contractelor cu furnizorii și de a urmări tranzacțiile de cereale între aceștia și clienți.
Caracteristicile și funcționalitățile cheie ale aplicației includ:
    Gestionarea furnizorilor: Utilizatorul pot adăuga și gestiona detalii despre furnizori, inclusiv informații despre companie, persoane de contact și date de contact. Aceasta permite o colaborare ușoară cu furnizorii agricoli.

    Gestionarea clienților: Aplicația permite administrarea detaliilor despre clienți, inclusiv informații despre companie, persoane de contact și date de contact. Astfel, utilizatorii pot menține o bază de date actualizată a clienților lor agricoli.

    Adăugarea contractelor: Utilizatorul pot crea și gestiona contracte cu furnizorii pentru a stabili detalii specifice ale tranzacțiilor, cum ar fi tipul de cereale, cantitatea, prețul și termenul de livrare.

O astfel de aplicatie rezolva gestionarea tranzactiilor pentru un patron de terenuri ce se ocupa constant de achizitionarea de noi
bunuri(multiple tipuri de cereale). Utilizatorul aplicatiei beneficiaza astfel de o impartire gestionata a produselor sale, separare intre partea de clienti, furnizori si cereale.

Descriere API:
Pentru baza de date am folosit FireBase. Am integrat in aplicatie un pachet care integreaza firebase api. Am ales FireBase datorita costurilor inexistente in cazul unei aplicatii cu un numar redus de utilizatori

Pentru validarea numarului de telefon am folosit ApiStack, un provider de servicii care ofera si generare de One Time Password printr-un request ce contine numarul de telefon al utilizatorului. Aceasta validare este esentiala din motive de securitate, pentru a limita accesul altor utilizatori. 


Flux de date:
Exemplu de send request catre ApiStack:

Future<void> submit() async {
    if (!validateForm()) {
      return;
    }

    print('start');

    Map<String, String> headers = {
      'Content-Type': 'application/json',
      'x-as-apikey': 'a61ef09b-fe42-4a12-ae57-b00bb1f087d9',
    };

    Map body = {
      "messageFormat":
          "Hello, this is your OTP. Please do not share it with anyone",
      "phoneNumber": telefonController.text,
      "otpLength": 6,
      "otpValidityInSeconds": 240,
    };

    HttpClient client = HttpClient();
    String serverUrl = 'www.getapistack.com';

    try {
      Map response = await client.post(
          Uri.https(serverUrl, '/api/v1/otp/send'), body, headers);
      setState(() {
        requestId = response['data']['data']['requestId'];
        hasSend = true;
        hasError = false;
      });
    } catch (e) {
      print(e.toString());
      print('hellooo');
      setState(() {
        hasSend = false;
        requestId = null;
        hasError = true;
      });
    }
}


Exemplu de validare request:
Future<void> submit2() async {
    if (!validateForm()) {
      return;
    }

    Map<String, String> headers = {
      'Content-Type': 'application/json',
      'x-as-apikey': 'a61ef09b-fe42-4a12-ae57-b00bb1f087d9',
    };

    Map body = {
      "otp": codSmsController.text,
      "requestId": requestId,
    };

    HttpClient client = HttpClient();
    String serverUrl = 'www.getapistack.com';

    try {
      Map response = await client.post(
          Uri.https(serverUrl, '/api/v1/otp/verify'), body, headers);
      setState(() {
        isOk = response['data']['data']['isOtpValid'];

        if (isOk) {
          hasError = false;
          widget.change();
        }
      });
    } catch (e) {
      setState(() {
        isOk = false;
        hasError = true;
      });
      print(e.toString());
    }
  }


Capturi ecran aplicație:
![image](https://github.com/DorobantuAndrei/proiectCloud/assets/84039610/75427cd4-00b3-440c-a9c5-6cfdc0b12f18)
![image](https://github.com/DorobantuAndrei/proiectCloud/assets/84039610/34892ea0-c764-4a47-802c-6e8d8689f791)
![image](https://github.com/DorobantuAndrei/proiectCloud/assets/84039610/1f1a111b-87b3-4dea-9694-dcdd1fcb0942)
![image](https://github.com/DorobantuAndrei/proiectCloud/assets/84039610/754e83e4-aae6-45e7-915c-1350db241fef)
![image](https://github.com/DorobantuAndrei/proiectCloud/assets/84039610/fd52c4d1-e2e6-4d21-bbf0-da7f30785e60)
