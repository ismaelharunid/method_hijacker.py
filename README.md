# method_hijacker.py
A couple of functions to hijack instance methods for those cases when you want to import functionality from an external object.

An Example Usage:

```python

from method_hijacker import create_proxy_class, define_proxy_method, NOT_RECOMMENDED

class Health(object):
  pass

class Feeling(object):
  _feeling = None
  def __init__(self, feeling='uncertain'):
    self._feeling = feeling
  
  @property
  def feeling(self): return self._feeling
  
  @feeling.setter
  def feeling(self, feeling): self._feeling = feeling
  
  def question(self, who=None):
    if who is None:
      return 'Are you feeling {:s}?'.format(self._feeling)
    return 'Is {:s} feeling {:s}?'.format(who, self._feeling)
  
  def answer(self, who=None):
    if who is None:
      return 'I\'m feeling {:s}.'.format(self._feeling)
    return '{:s} is feeling {:s}.'.format(who, self._feeling)


ProxyHealth = create_proxy_class('ProxyHealth', Health, '_feeling_proxy' \
    , Feeling, '_feeling,feeling,question,answer')

h = ProxyHealth()
assert h.feeling == 'uncertain'
h.feeling = 'happy'
assert h.feeling == 'happy'
assert h.question() == 'Are you feeling happy?'
assert h._feeling_proxy._feeling == 'happy'
assert h._feeling == 'happy'
h._feeling = 'sad'
assert h.feeling == 'sad'
assert h.question() == 'Are you feeling sad?'
assert h.question('Lia') == 'Is Lia feeling sad?'
```

