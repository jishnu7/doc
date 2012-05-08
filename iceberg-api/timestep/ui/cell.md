# `timestep.ui.Cell`

## Inheritence

1. [timestep.View](../view.md)
2. [lib.PubSub](../../lib/pubsub.md)

## Methods

* __remove__

* __getHeight__

	@return `{number}` ---Returns `this.style.height`.

* __getWidth__

	@return `{number}` ---Returns `this.style.width`.

* __setData (data)__ ---This needs to be redefined if you
  want to use this data in your view.

	@param `{object}` data

* __setPosition (pos)__

	@param `{}` pos


## Properties

* __model__ `{squill.models.Cell}` ---[`squill.models.Cell`](../../squill/models/cell.md).


## Messages

`Recycle` ---When `this.model` receives a `Recycle` event, this
`Cell` is removed from it's superview.