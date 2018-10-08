# injector

## Generate a usable

### Usage

Install:

`go get -u github.com/mpobrien/injector`

Let's say you have some interface in your code, like:

```go
type ThingDoer interface {
	Do(count int) (string, error)
}
```

When testing, it can be useful to have a _mock_ implementation of this interface.
For example, you may want to test that your code properly handles when an error is returned from `Do()`.

To generate a mock implementation, add a comment near the type:
`//go:generate inject -type=ThingDoer`

This will generate a mock implemenation into `thingdoer_inject.go`.
In your test code, you can now do:

```go
testThingDoer := &InjectThingDoer{}
testThingDoer.InjectDo = func(count int) (string, error){
	return "", errors.New("oops!")
}

// any code that expects a ThingDoer as argument can now also receive *InjectThingDoer.
runSomeCode(testThingDoer)
```
