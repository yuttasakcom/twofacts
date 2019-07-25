# TwoFacts
Two-factor authentication for Node.js using one-time password (TOTP, HOTP). Supports Google Authenticator app.

TwoFacts implements the HMAC-Based One-time Password (HOTP) algorithm specified in [RFC 4226](https://tools.ietf.org/html/rfc4226) and the Time-based One-time Password (TOTP) algorithm specified in [RFC 6238](https://tools.ietf.org/html/rfc6238).



## APIs
### verifyUsingGoogleAuthenticator
Verifies if the OTP entered by user is correct or not.

Each parameter this function accepts is decribed below:

| **Parameter** | **Required?** | **Default Value**  | **Description**                                                      |
| ------------- | ------------- | ------------------ | -------------------------------------------------------------------- |
| `otp`         |      Yes      |       None         | One-time password user has entered from the Google Authenticator app.|
| `secret`      |      Yes      |       None         | The secret key for the application against which OTP is generated.   |                                                                                  

#### Returns 
`Boolean` value `true` or `false` indicating if the `otp` entered by user was valid or not.

---

### generateSecret
Generates a new, random, base32-encoded secret key that can be added to an authenticator app, like Google Authenticator, to generate OTPs.

Each parameter this function accepts is decribed below:

| **Parameter** | **Required?** | **Default Value**  | **Description**                            |
| ------------- | ------------- | ------------------ | ------------------------------------------ |
| `length`      |      No       |       `20`         | Length of the secret key.                  |

#### Returns 
Base32 encoded random `string` value as the secret.

---

### verifyTOTP
Verifies if the Time-based OTP entered by user is correct or not.

Each parameter this function accepts is decribed below:

| **Parameter**       | **Required?** | **Default Value**  | **Description**                                                      |
| -------------       | ------------- | ------------------ | -------------------------------------------------------------------- |
| `otp`               |      Yes      |       None         | One-time password user has entered from the Google Authenticator app.|
| `secret`            |      Yes      |       None         | The secret key for the application against which OTP is generated.   |                                                                                  
| `window`            |      No       |       `1`          | The time step window for which the function should verify. For example, if the time step is 30 seconds and window is 1, then the function will verify OTPs generated at current time, 30 seconds before current time and 30 seconds after current time. If window is 2, it will check for 60 seconds before current time and 60 seconds after. |
| `timeStepInSeconds` |      No       |       `30`         | The size of window in seconds after which a new OTP is calculated.   |
| `initialTime`       |      No       |       `0`          | The starting time which should be subtracted from current time to verify OTP. Default value `0` indicates the Unix epoch time (00:00:00 January 1st, 1970 at UTC).   |
| `otpLength`         |      No       |       `6`          | The number of digits of OTP generated.                               |

#### Returns 
`Boolean` value `true` or `false` indicating if the `otp` entered by user was valid or not.

---

### generateTOTP
Generates a Time-based one time password.

Each parameter this function accepts is decribed below:

| **Parameter**       | **Required?** | **Default Value**  | **Description**                                                             |
| -------------       | ------------- | ------------------ | --------------------------------------------------------------------        |
| `secret`            |      Yes      |       None         | The secret key for the application against which OTP should be generated.   |                                                                                  
| `window`            |      No       |       `0`          | The time step window for which the function should generate the OTP. If window is `0`, the OTP will be generated at the current time. If window is `-1`, then the OTP will be generated at `current time - 30 seconds`. |
| `timeStepInSeconds` |      No       |       `30`         | The size of window in seconds after which the OTP is generated.             |
| `initialTime`       |      No       |       `0`          | The starting time which should be subtracted from current time to verify OTP. Default value `0` indicates the Unix epoch time (00:00:00 January 1st, 1970 at UTC).   |
| `otpLength`         |      No       |       `6`          | The number of digits of OTP generated.                               |

#### Returns
`Number` value which will be the time-based one time password for a secret.

---

### generateHOTP
Generates a HMAC-based one time password.

Each parameter this function accepts is decribed below:

| **Parameter**       | **Required?** | **Default Value**  | **Description**                                                             |
| -------------       | ------------- | ------------------ | --------------------------------------------------------------------        |
| `secret`            |      Yes      |       None         | The secret key for the application against which OTP should be generated.   |                                                                                  
| `counter`           |      Yes      |       None         | The counter required for HOTP generation.                                   |
| `otpLength`         |      No       |       `6`          | The number of digits of OTP generated.                                      |
| `hmacAlgorithm`     |      No       |       `sha1`       | The algorithm to use to create HMAC. Examples are `sha256`, `sha512`.       |

#### Returns
`Number` value which will be the hmac-based one time password for a secret.