# 淡江選課系統 Chrome 自動輸入驗證碼

## How to?

1. Change the variable `username` to your studend ID. *(`const username = '123456'`)*
2. Change the variable `password` to your SSO password. *(`const password = 'password'`)*
3. Create a bookmark in your Chrome and copy the following code to the field named `URL`
4. Go to [`http://www.ais.tku.edu.tw/EleCos/login.aspx`](http://www.ais.tku.edu.tw/EleCos/login.aspx) or [`https://www.ais.tku.edu.tw/EleCos_English/loginE.aspx`](https://www.ais.tku.edu.tw/EleCos_English/loginE.aspx) then click the bookmark which you just created.

## Code

```javascript
javascript: {
    const url = 'Handler1.ashx';
    const username = '123456';
    const password = 'password';
    const hash_to_number = {
        "b6589fc6ab0dc82cf12099d1c2d40ab994e8410c": 0,
        "356a192b7913b04c54574d18c28d46e6395428ab": 1,
        "da4b9237bacccdf19c0760cab7aec4a8359010b0": 2,
        "77de68daecd823babbb58edb1c8e14d7106e83bb": 3,
        "1b6453892473a467d07372d45eb05abc2031647a": 4,
        "ac3478d69a3c81fa62e60f5c3696165a4e5e6ac4": 5,
        "c1dfd96eea8cc2b62785275bca38ac261256e278": 6,
        "902ba3cda1883801594b6e1b452790cc53948fda": 7,
        "fe5dbbcea5ce7e2988b8c69bcfdfde8904aabc1f": 8,
        "0ade7c2cf97f75d009975f4d720d1fa6c19f4897": 9,
    };
    const request = new XMLHttpRequest();
    const login_form = window.document.forms['form1'];
    login_form.txtStuNo.value = username;
    login_form.txtPSWD.value = password;
    request.onreadystatechange = function () {
        if (this.readyState == 4) {
            console.log('status code:', this.status);
            if (this.status == 200) {
                try {
                    const code = JSON.parse(this.responseText).map(x => hash_to_number[x]).join('');
                    login_form.txtCONFM.value = code;
                }
                catch (e) { console.log(e); }
            }
            __doPostBack('btnLogin', '');
        }
    };
    request.open('GET', url, true);
    request.send();
}
```
