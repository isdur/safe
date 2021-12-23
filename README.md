# safe
password encrypted so can be public


* -------------- WHEN ITS SETUP ALREADY ....................
# for alpine, add openssh git zip
docker run --rm -ti alpine sh
apk add git openssh gnupg # 38mb
git clone https://github.com/isdur/safe.git && cd safe
gpg --cipher-algo AES256 -o blob.tar.gz -d blob.tar.gz.gpg
tar -xvf blob.tar.gz
mkdir ~/.ssh && cp id_ed25519 ~/.ssh/.

# then remove everything.
# now you can pull ssh repose and push ssh repos too.
* ........................................

** dont use rsa for ssh keys, is slower and less safe than ed25519, its just that its more compliant with old systems.
** change key user git config user.name "keymaster" && git config user.email "keymail"
** brute force protecting a ssh private key ssh-keygen -t ed25519 -a 100        # the -a 100 higher number means slower password validation hence harder to brute force.
* github now only supports ssh keys. only open repos can be cloned with https and no key.
** cloneing a pulbic repo is with https, but pushing has to be with ssh i.e git@github address.
** cloening without a key has to be done with https.

* HOW THIS WAS CRATED: in a container:
** 1 create a new key, from a new system , then throw away the system.
  git clone https://github.com/isdur/safe.git
  ssh-keygen -t ed25519 # in ubuntu its there, in alpine do apk add openssh
  cat ~/.ssh/id_ed25519.pub # and paste into github.
  
  tar(used to make mutlie files a singlefile).gz(zio) and gpg(encrypt) the private key.
mkdir tmpk && cd tmpk
# copy the new private key from the home ssh folder
cp ~/.ssh/id_ed25519* .
tar -czvf blob.tar.gz id_ed25519
gpg --no-symkey-cache -c blob.tar.gz #TODO might be good to set the cypher metod here too.

mv the blob.tar.gz.gpg to the repo, and add commit push it.

# yopu have to set the repo to be the git@github instead of https, since github does not support pushing with https anymore(august 2021).
git remote set-url origin git@github.com:isdur/safe.git


