---
title: "Keep your npm packages up to date with ncu"
description: "Describes how to check and update package versions in a package.json file using a CLI tool called npm-check-updates."
author: "Jan-Philipp Kempf"
pubDate: "2023-08-18T17:00:00+02:00"
updatedDate: "2023-10-11T11:32:00+02:00"
---

[npm-check-updates](https://www.npmjs.com/package/npm-check-updates) is a handy CLI tool that checks your `package.json` lists the current and latest versions for any of your installed packages that aren't already up to date.

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

If you want to update _every_ package from that list to its latest version in one go, you can do this:

```bash
npx npm-check-updates -u
```

However, this becomes less and less useful the more dependencies you have. You might not want to do all major version bumps in one go, or you might have packages that you know you can't or don't want to update right now. Enter interactive mode, ideally paired with the `--format` option:

```bash
npx npm-check-updates -i --format group
```

This lets you choose which dependencies you want to upgrade and which ones you'd rather skip for now. Selecting `group` as your format will group dependencies by patch, minor and major version bumps and pre-select the options that are likely safe to update.

Finally, ncu will prompt you to run `npm install` to actually install the new versions and update your lock file.
