### My Library: (games/cards) a Virtual Playing Cards Library

  Our group determine to make a card game in racket, so I explored the  (games/cards) library. I use the library for for creating card games. So the first step is to create a table,The table is named by Memory, and it is w cards wide and h cards high.(w=4, h=2). Then creating my-deck by (make-deck). And I add the cards in the table.When running the code, it can show a window (picture4). There are 4 cards in the window, The next step I plan to do the flapping part, that means the operator can flap the card.

#Code:
lang racket
(require games/cards)
(require racket/gui)

(define WIDTH 4)
(define HEIGHT 2)
(define t (make-table "Memory" (+ 2 WIDTH) (+ 1 HEIGHT)))
(send t set-double-click-action #f)

(define w (send t table-width))
(define h (send t table-height))

(define deck 
  (let ([cards (map (lambda (name value)
                      (let ([bm (make-object
                                 bitmap%
                                 (build-path
                                  (collection-path "games" "memory" "images")
                                  (format "~a.png" name)))])
                        (make-card bm #f 0 value)))
                    '("club" "heart" "spade" "diamond"
                      )
                    '(1 2 3 4 ))])
    (append cards (map (lambda (c) (send c copy)) cards))))
  ( for-each (lambda (card)
            (send card user-can-move #f)
            (send card user-can-flip #t))
          deck)


(define cw (send (car deck) card-width))
(define ch (send (car deck) card-height))
(define dx (/ cw (+ 2 WIDTH)))
(define dy (/ ch (+ 1 HEIGHT)))


(define match-x (- w cw dx))
(define match-y dy)


(send t add-cards deck match-x match-y)


(define (setup)
  (set! deck (shuffle-list deck 7))
  (send t stack-cards deck)
  (send t move-cards deck 0 0
        (lambda (pos)
          (let ([i (modulo pos WIDTH)]
                [j (quotient pos WIDTH)])
            (values (+ dx (* i (+ cw dx)))
                    (+ dy (* j (+ ch dy))))))))
                    
(setup)
(send t show #t)





1. To start, [**fork** this repository][forking].
1. Modify the README.md file and [**commit**][ref-commit] changes to complete your solution.
  2. (This assignment is just one README.md file, so you can edit it right in github without cloning)
  3. (You may need to clone and push if you want to add extra files)
1. [Create a **pull request**][pull-request] on the original repository to turn in the assignment.


