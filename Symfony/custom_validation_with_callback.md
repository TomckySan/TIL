カスタムバリデーションの実装で個人的によく使うやり方はコレ。

```php
// ...
use Symfony\Component\Validator\Constraints\Callback;
use Symfony\Component\Validator\Context\ExecutionContextInterface;
use App\Entity\Post;

class PostType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        // ...
    }

    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults([
            'data_class' => Post::class,
            'constraints' => [
                new Callback(function (Post $post, ExecutionContextInterface $context) {
                    if ($post->getPublicStartTime() > $post->getPublicEndTime()) {
                        $context->buildViolation('Cannot over start time.')
                            ->atPath('publicEndTime')
                            ->addViolation()
                        ;
                    }
                }),
            ],
        ]);
    }
}
```

関連するSymfonyのドキュメント  
[Constraints/Callback](https://symfony.com/doc/current/reference/constraints/Callback.html)

他の方法についても色々調べた。

https://symfonycasts.com/screencast/question-answer-day/custom-validation-property-path  
これは少々古い。例えばSymfony3.3では動作しない。

まず、addViolationAtはdeprecatedになっている。  
https://stackoverflow.com/questions/25264922/symfony-2-5-addviolationat-deprecated-use-buildviolation  
従ってbuildViolationを使って記述する必要がある。  

あと次の記述でもエラーが出る。

```php
/**
 * @Assert\Callback(methods={"checkCustomValidation"})
 */
 class Event
 {
```

以下エラーの内容。

```
{"methods":["checkCustomValidation"]} targeted by Callback constraint is not a valid callable
```

エラー発生箇所は `Symfony/Component/Validator/Constraints/CallbackValidator.php` でソースコードは以下のようになっている。

```php
    public function validate($object, Constraint $constraint)
    {
        if (!$constraint instanceof Callback) {
            throw new UnexpectedTypeException($constraint, __NAMESPACE__.'\Callback');
        }

        $method = $constraint->callback;
        if ($method instanceof \Closure) {
            $method($object, $this->context, $constraint->payload);
        } elseif (is_array($method)) {
            if (!is_callable($method)) {
                if (isset($method[0]) && is_object($method[0])) {
                    $method[0] = get_class($method[0]);
                }
                throw new ConstraintDefinitionException(sprintf('%s targeted by Callback constraint is not a valid callable', json_encode($method)));
            }

            call_user_func($method, $object, $this->context, $constraint->payload);
        } elseif (null !== $object) {
```

`$method` には `['methods' => ['checkCustomValidationA']];` という配列が入っている。  
これを動かすためにはこうしないといけない。

```php
/**
 * @Assert\Callback("checkCustomValidation")
 */
 class Event
 {
```

また、

```php
use Symfony\Component\Validator\ExecutionContextInterface;
```

ではなく、

```php
use Symfony\Component\Validator\Context\ExecutionContextInterface;
```

でないといけない。

これで一番最初に解説されているEntityにCallbackを記述したコードが動く。