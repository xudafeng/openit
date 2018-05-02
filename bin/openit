#!/usr/bin/env node

'use strict';

const fs = require('fs');
const _ = require('xutil');
const path = require('path');
const EOL = require('os').EOL;
const child_process = require('child_process');

const cwd = process.cwd();
const platform = process.platform;

const linuxShell = platform === 'linux' ? 'xdg-open' : 'open';
const openShell = platform === 'win32' ? 'start' : linuxShell;

const git_config = path.join(cwd, '.git', 'config');
const git_config_second = path.join(cwd, '..', '.git', 'config');
const git_config_third = path.join(cwd, '..', '..', '.git', 'config');

var get_remote_url = content => {
  const list = content.split(EOL);
  var res;
  list.forEach(item => {
    item = item.trim();
    var str = item.slice(0, 10);

    if (str === 'url = git@' || str === 'url = http') {
      const _list = item.split(' ');
      const dist = _list[_list.length - 1];
      if (dist.slice(0, 4) === 'git@') {
        res = dist.replace(':', '/').replace('git@', 'http://');
      } else {
        res = dist;
      }
      res = res.replace(/\.git$/, '');
    }
  });
  return res;
};

_.forEach([
  git_config,
  git_config_second,
  git_config_third
], git_config => {
  if (_.isExistedFile(git_config)) {
    const content = fs.readFileSync(git_config, 'utf8');
    const url = get_remote_url(content);

    if (!url) {
      console.log('git remote url not exist');
      return true;
    }
    child_process.execSync(`${openShell} ${url}`);
    return false;
  }
  return true;
});
