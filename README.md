# Repro for issue 2843

## Versions

firebase-tools: v13.30.0<br>
firebase-admin: v13.1.0<br>
node: v22.13.1<br>
platform: macOS Sonoma 14.7.3

## Steps to reproduce

1. Install dependencies
   - Run `cd admin-app`
   - Run `npm i`
   - Run `cd ../`
2. Run `firebase emulators:start --project demo-project`
3. Open a new terminal
   - Run `cd admin-app`
   - Run `node index.js`
   - Errors with:

```
$ node index.js
(node:49759) [DEP0040] DeprecationWarning: The `punycode` module is deprecated. Please use a userland alternative instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
/<PATH>/new-report/admin-app/node_modules/firebase-admin/lib/utils/api-request.js:268
            throw new error_1.FirebaseAppError(error_1.AppErrorCodes.NETWORK_ERROR, `Error while making request: ${err.message}. Error code: ${err.code}`);
                  ^

FirebaseAppError: Error while making request: write EPROTO 808F8EEE01000000:error:0A00010B:SSL routines:ssl3_get_record:wrong version number:../deps/openssl/openssl/ssl/record/ssl3_record.c:355:
. Error code: EPROTO
    at /<PATH>/new-report/admin-app/node_modules/firebase-admin/lib/utils/api-request.js:268:19
    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)
    at async /<PATH>/new-report/admin-app/node_modules/firebase-admin/lib/data-connect/data-connect-api-client-internal.js:91:26
    at async main (file:///<PATH>/new-report/admin-app/index.js:20:25) {
  errorInfo: {
    code: 'app/network-error',
    message: 'Error while making request: write EPROTO 808F8EEE01000000:error:0A00010B:SSL routines:ssl3_get_record:wrong version number:../deps/openssl/openssl/ssl/record/ssl3_record.c:355:\n' +
      '. Error code: EPROTO'
  },
  codePrefix: 'app'
}
```

### Expected behavior

Running `node index.js` should output:

```
{"data":{"movies":[]}}
```

## Notes

Changing `DATA_CONNECT_EMULATOR_HOST = "127.0.0.1:9399"` to `DATA_CONNECT_EMULATOR_HOST = "http://127.0.0.1:9399"` resolves the issue.
