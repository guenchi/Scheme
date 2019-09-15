# Emacs support for (Petite) Chez Scheme

If you're running Linux, you already have Emacs. If you're running some other Unix variant, you can obtain it from <http://www.gnu.org/software/emacs>. If you're running Windows, you can obtain it from <http://www.gnu.org/software/emacs/windows/ntemacs.html> and get additional help for installing Emacs on Windows from <http://w3.trib.com/~ccurley/emacs.html>.

There are three levels of customization you might want to run Chez Scheme or Petite Chez Scheme under Emacs: [quick and dirty](#quickanddirty), [iuscheme](#iuscheme), or [insane](#insane). If you are running Petite Chez Scheme instead of Chez Scheme, you'll need to make a [small additional customization](#smalladditionalcustomization).

<a name="quickanddirty"></a>
### Quick and Dirty Setup

If you're not yet comfortable with Emacs and you don't want to make your emacs very customized (in order to stay somewhat standard), just add the following three lines to your emacs configuration file

```
(autoload 'scheme-mode "cmuscheme" "Major mode for Scheme." t)
(autoload 'run-scheme "cmuscheme" "Switch to interactive Scheme buffer." t)
(setq auto-mode-alist (cons '("\\.ss" . scheme-mode) auto-mode-alist))
```

Under Unix, your emacs configuration file is a file in your home directory called **.emacs** (that's pronounced "dot-emacs"; don't forget the dot). Under Win32 systems, it's a file in your home directory (that's the directory pointed to by environment variable %HOME%, or **C:**\ if there is no such variable) called either **.emacs** or **_emacs** ("underscore-emacs").

<a name="iuscheme"></a>
### IUScheme Setup

Those of us who (fortunately or unfortunately) have to write a lot of Scheme code have made a few changes to the default Scheme mode in Emacs, and put together a package called [iuscheme.el](https://web.archive.org/web/20151022142254/http://www.cs.indiana.edu/chezscheme/emacs/iuscheme.el). If you want to use it, first make sure you have a place to put emacs-lisp files (which have the extension ".el"). Assuming you've chosen to place them in the directory **~/emacs**, download the package, place it there, and make sure you have the following expression in your emacs configuration file:

```
(setq load-path (cons "~/emacs" load-path))
```

Then, all you need to do is make sure these three lines are in your configuration file

```
(autoload 'scheme-mode "iuscheme" "Major mode for Scheme." t)
(autoload 'run-scheme "iuscheme" "Switch to interactive Scheme buffer." t)
(setq auto-mode-alist (cons '("\\.ss" . scheme-mode) auto-mode-alist))
```

Also, you might want to add a few indentation declarations to your configuration file, via

```
(put 'form-id 'scheme-indent-function number-of-special-subforms)
```

where the number of special subforms is something like 1 for when, because the first subform is required, and 2 for syntax-case.

<a name="insane"></a>
### Insane Setup

If you write as much Scheme code as some of us do, you may be willing to undergo behaviour modification if it would allow you to never make an unbalanced-parenthesis-type error ever again. If this sounds like a good idea to you, then first follow the directions for the IUScheme setup. Then, download [balanced.el](https://web.archive.org/web/20160826073229/http://www.cs.indiana.edu/chezscheme/emacs/balanced.el), stick it in your emacs-lisp directory, and add the following lines to your configuration file

```
(autoload 'balanced-toggle "balanced" "Toggle balanced ``mode''" t)
(autoload 'balanced-on "balanced" "Turn on balanced ``mode''" t)
(add-hook 'scheme-mode-hook 'balanced-on)
```

Now, every time you insert a left parenthesis while working on a Scheme file, the matching right parenthesis will automatically be inserted. Hitting a right parenthesis will move you past the next right parenthesis. "M-1 (" (Meta-one, left parenthesis) will surround the next "s-expression". Etc, etc.

If you use this, you'll want to brush up on M-C-f, M-C-b, M-C-u, M-C-d, M-C-k, and M-C-space.

<a name="smalladditionalcustomization"></a>
### Petite Chez Scheme

By default, Emacs will invoke "scheme" each time you type "M-x run-scheme". If you don't have Chez Scheme, but do have Petite Chez Scheme, you'll want it to invoke "petite" instead. To do this, include the following line in your emacs configuration file.

```
(custom-set-variables '(scheme-program-name "petite"))
```

If this doesn't work in your version of Emacs, you might try

```
(setq scheme-program-name "petite")
```

instead. On a Windows 95/98/ME system, you may have to use "petite.bat" or even "c:/windows/petite.bat" in place of "petite".

---

Acknowledgement: Most of the information on this page was copied almost verbatim from Erik Hilsdale's C311 resources page at Indiana University.