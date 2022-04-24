# AUR-wrapper

A small Python script that attempts to launch a binary and tells the user to rebuild it if it fails.

## Usage

In the AUR package's PKGBUILD:

1. Add `python3` and `python-pyqt5` to the `depends` array.
2. Add AUR-wrapper to the `source` array.
3. Inside the `package()` function, after the original package is installed, move the original binary to my_program.real and install AUR_wrapper.py as my_program.

An example would be:
```bash

# ...

depends=('my_depends' 'python3' 'python-pyqt5')

# ...

source=('my_source'
        'AUR-wrapper::git+https://github.com/ckb-next/AUR-wrapper.git')

# ...

package() {
  cd "$srcdir/${pkgname%-VCS}"

  DESTDIR="$pkgdir" cmake --build build --target install

  # Rename the real binary and add the wrapper
  mv "$pkgdir/usr/bin/my_program" "$pkgdir/usr/bin/my_program.real"

  cd "$srcdir/AUR-wrapper"
  install -Dm755 AUR_wrapper.py "$pkgdir/usr/bin/my_program"
}
```