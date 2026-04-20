# Troubleshooting

*Because Beau and Chris can't be everywhere at once*

## Errors installing

*Nothing yet*

## Errors running heft

### ExceptionCode=-1073741819, ExceptionFlags=0, ExceptionAddress=0F8112F8 version=XXX on *"windows_ia32"*  pc=f8112f8 Stack dump aborted because GetAndValidateThreadStackBounds failed.

Uh oh, are you running on a Snapdragon processor?

#### Diagnosis

1. In your terminal, try running the following command:

   ```bash
   node -p "process.arch"
   ```

   If you get a response that has `ia32` in it, you're trying to run on a version of Node.js that isn't compatible

#### Resolution

1. Add a compatible version of Node.js:

   ```bash
   nvs add 22/x64
   nvs use 22/x64
   ```
1. If you had previously run `npm i`, delete the `node_modules` folder and `package-lock.json` file
1. Install dependencies:

    ```bash
    npm i
    npm install -D sass-embedded sass-embedded-win32-x64
    ```
1. Proceed with your `heft` command as per the labs:
   ```bash
   heft start
   ```
