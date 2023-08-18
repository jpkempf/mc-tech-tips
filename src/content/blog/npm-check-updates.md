---
title: "Keep your npm packages up to date with ncu"
description: "Describes how to check and update package versions in a package.json file using a CLI tool called npm-check-updates."
pubDate: "2023-08-18T17:00:00+02:00"
---

`npm-check-updates` is a handy CLI tool that checks your `package.json` lists the current and latest versions for any of your installed packages that aren't already up to date.

#### Executing the command

You can run the tool without installing it locally via `npx`:

```bash
npx npm-check-updates
```

You can also install the package globally, which will allow you to run it anywhere without the `npx` prefix:

```bash
npm install -g npm-check-updates
```

Then you can call it either as `npm-check-updates` or with the handy `ncu` alias that comes with the package.

#### Checking for updates

When you execute `npm-check-updates` in a folder that contains a `package.json` file, it will give you some output like in the example below:

![Terminal output showing a list of packages. Each package in the list has it's current version and the latest available version standing next to it.](/mc-tech-tips/npm-check-updates-output.webp)</figure>

Note the different color coding in the output: green for patches, blue for minor updates and red for major updates that might contain breaking changes.

#### Applying updates

If you want to update every package from that list to its latest version in one go, you can do this:

```bash
npx npm-check-updates -u
```

> Use this command with caution, as it might cause your application to break, especially if some of your packages are multiple versions behind!

Be sure to test thoroughly that everything still works afterwards, and consult the release notes of the packages that were updated to see if there are any breaking changes. Alternatively, you can also choose to update individual packages:

```bash
npx npm-check-updates -u <package-name>
```

This will update your `package.json` file with the new version numbers.

Note that you will still need to run `npm install` to actually install the new versions and update your lock file, if you have one!
