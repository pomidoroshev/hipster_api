# hipster_api
wrapper django rest framework

## Применение
Для удобства использования restapi в вашем проекте

Подключение
```python
INSTALLED_APPS = (
...
'rest_framework',
'hipster_api',
)
TEMPLATE_DIRS = (
    os.path.join(os.path.dirname(__file__), '..', 'templates').replace('\\', '/'),
)
```


## Пример views
```python
from hipster_api import fields as hfields
from hipster_api.views import HView


class QuestsView(HView):
    """
    Работа с постами
    """

    class Fields(object):
        fields = hfields.FieldsListResponse(verbose_name=u'Список полей через запятую', methods=['get'])
        offset = hfields.IntegerLarger(default=0, larger=0, glt=True, methods=['get'])
        limit = hfields.IntegerLarger(default=20, larger=0, methods=['get'])
        active = hfields.IntegerList(default=u'0,1', methods=['get'])
        
        name = hfields.String(verbose_name=u'Название поста', default=u'', methods=['put'])
        description = hfields.String(verbose_name=u'Название поста', default=u'', methods=['put'])

    def get(self, request, format=None):
        """
        Получение постов
        :public:
        """
        fields = ['id'] + self.objects.fields
        posts = Post.objects.values(*fields).filter(active__in=self.objects.active)[self.objects.offset:self.objects.limit]
        
        return Response(quests)
        
    def put(self, request, format=None):
        """
        Создание поста
        :private только админа:
        """
        
        Post(name=self.objects.name, description=self.objects.description).save()
        return Response(status=202)
        
```

### запросы
```
GET /api/v1/posts.json?fields=name
GET /api/v1/posts.json?fields=name,description

PUT /api/v1/posts.json
name = 'тест'
description = 'тес тест'

```

# реализованные поля

* [x] String
* [x] StringList
* [x] FieldsListResponse
* [x] Integer
* [x] IntegerLarger
* [x] IntegerList
* [x] IntegerLess
* [x] Float
* [x] FloatLess
* [x] FloatLarger
* [x] FloatList
* [x] Boolean
* [x] DateTime
* [x] Date
* [x] JsonField
