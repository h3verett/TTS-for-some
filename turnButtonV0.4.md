
TurnButton.md

An attempt to isolate turn management from game to enable drop-in operation

Please let me know any ideas or issues you find with this.

This version assumes that the Global UI will not change the order or position of the turnButton entries.  A future version will address this.
This version drops into your global script. It defines these globals:

function tB_curPlayer() -- need this only if you want to use curPlayer in another object.
-- work arounds due to requirement that all button callbacks functions must be top level.
function tB_start(player, value, id)
function tB_endTurn(player, value, id)
function tB_beginMoveButton(player, value, id)
function tB_endMoveUI(player, value, id)
function tB_toggleGlobalPanel(player, value, id)
function tB_config(player, value, id)
function tB_colorSelect(player, value, id)
function tB_setOrder(player, value, id)
function tB_closeIt(player, value, id)

tB -- table that contains the bulk of the turnButton data AND code tB_start -- just calls tB.start; work around for broken UI interface to TTS tB_endTurn -- just calls tB.endTurn; work around for broken UI interface to TTS tB_endMoveUI -- just calls tB.endMoveUI; work around for broken UI interface to TTS tB_toggleGlobalPanel -- just calls tB.toggleGlobalPanel; work around for broken UI interface to TTS doNothing -- just does nothing; work around for broken UI interface to TTS

API

tB.curPlayer -- string color of current player; but use tb.set() to change it

tB.order -- table of strings used as a circular array; you may alter as you like, but should have at least one element.

tB.init() -- initialize the turnButton with two buttons:
  "HotSeat Setup"
    brings up a setup window with all possible colors in left column -- colors with corresponding seats are darkened.
    left click a color in left column to add it to the hotseat list in right column
    right click a color in right column to remove from hotseat list.
    press "set Order" to confirm your choices.  (game does not start; you can re-edit if you click by mistake.
    
  "Start the game"
    completes turnButton initialization, setting order to hotseat array if you configured it, or to getSeatedPlayers() sorted
    into clockwise order around the table. Note this may seem backward if all players are at the "front" of the table. Edit
    to suit your need.
    Chooses a first player at random. Produces an error if there are no seated players.
    Calls global function initGame, if present.
    
tB.hotSeat -- table of players for use in hotSeat mode. Copied to order at start if it exists.
  Players need not be seated at start time, but will be required to sit before ending their turn.

tB.start(player) -- called from UI when a seated player clicks the start button currently sets order to getSeatedPlayers() that have hands, re-ordered by hand position around table

tB.next() -- returns player next in order array

tB.prev() -- returns player previous in order array

tB.set(newPlayer) -- set curPlayer to newPlayer if newPlayer is seated

tB.endTurn(player) -- target of UI player end turn button
  -- calls global onTurnEnd, if present, and processes end of turn only if onTurnEnd returns true.
  -- if hotSeat is configured, switches the seat of the player to the next entry in order.
  -- calls global onTurnStart, if present.
  
tB.toggleGlobalPanel() -- hide/show global buttons

tB.save() -- returns a table suitable for combining with other save data in global onSave()

tB.restore(tb.save()) -- takes a table made by tb.save and configures the turnButton accordingly

tB.pass() -- force end of turn

turnButton calls several routines in Global if they exist:
  initGame(player) -- when a player clicks one of the start buttons
  onTurnStart() -- when a player's turn is beginning
  onTurnEnd() -- when a player's turn is ended must return true if turn should end, false otherwise
