import math, pygame, random
from pygame.locals import *
clock=pygame.time.Clock()
pygame.init()
def drawShape(w,h,x,y,colour,shape):
    color=(colour[0],colour[1],colour[2])
    if shape==0:
        surf = pygame.Surface((w, h))
        surf.fill(color)
        rect = surf.get_rect()
        screen.blit(surf, (x, y))
    else:
        pygame.draw.circle(screen, color, (x, y), w/2)

def convertToBinList(numberTaker,length):
    binList=[int(i) for i in list("1"+bin(numberTaker))[3:]]
    return [0]*int(length-len(binList))+binList
def convertToID(binList):
    return sum([binList[i]*2**(len(binList)-i-1) for i in range(len(binList))])

def findSubsequentTape(tape):
    if rulestring==150:
        subsequentTape=[(tape[j-1]+tape[j]+tape[j+1])%2 for j in range(bound,len(tape)-bound)]
    else:
        for j in range(bound,len(tape)-bound):
            subsequentTape.append(rule[len(rule)-1-convertToID(tape[(j-bound):(j+bound+1)])])
    if cylindricalTape==1: #this part is inelegant and cheap but no-one needs to know
        if rulestring==150:
            subsequentTape=[(tape[-1]+sum(tape[0:2]))%2]+subsequentTape+[(sum(tape[-2:0])+tape[0])%2]
        else:
            subsequentTape=rule[len(rule)-1-convertToID([tape[-1],tape[0:2]])]+subsequentTape+rule[len(rule)-1-convertToID([tape[-2:0],tape[0]])] #doesn't seem to allow ranges through the seam
    else:
        subsequentTape=tape[0:bound]+subsequentTape+tape[(len(tape)-bound):len(tape)]
    return subsequentTape

def findLoopsPerLength(stateTransitions):
    loopLengths=[]
    statesInLoop=[0]*len(stateTransitions)
    for i in range(len(stateTransitions)):
        j=i
        currentLoop=[]
        while (currentLoop.count(j)==0 and statesInLoop[j]==0) or currentLoop==[]:
            currentLoop.append(j)
            statesInLoop[j]=1
            #print(j,stateTransitions[j])
            j=stateTransitions[j]
        if currentLoop.count(j)!=0:
            loopLengths.append(len(currentLoop)-currentLoop.index(j)+1)
    loopsPerLength=[int(loopLengths.count(i))*(1+(rulestring==150 and cylindricalTape==0)) for i in range(1,max(loopLengths)+1)]
    return loopsPerLength

def formatNicely(listToFormat):
    nice=[]
    for i in range(len(listToFormat)):
        if listToFormat[i]>0:
            nice.append([i,listToFormat[i]])
    return nice

def graphRLE(inputGraph):
    graphHeight=max([0 if i==[] else max([j[0] for j in i]) for i in inputGraph])
    graphToExport=[]
    for i in range(len(inputGraph)):
        line=[1]
        for j in range(len(inputGraph[i])-1):
            line+=[0]*(inputGraph[i][j+1][0]-inputGraph[i][j][0]-1)+[1]
        graphToExport.append(line)
    #print(graphToExport)
    RLE=""
    for i in graphToExport:
        im=i[0]
        k=0
        for j in i+[2]: #What an awful dream. Ones and zeros everywhere. And I think I saw a two.
            if j!=im:
                RLE+=str(k)*(k!=1)+("b" if im==0 else "o")
                im=j
                k=0
            k+=1
        RLE+="$"
    RLE="#C RLE graph of oscillator periods with respect to length (from "+str((bound*2+1)*(cylindricalTape==0))+" to "+str(len(inputGraph)+2)+") in bounded tapes in Wolfram rule "+str(rulestring)+'''
'''+"x = "+str(graphHeight)+", y = "+str(len(inputGraph))+", rule = B3/S23"+'''
'''+RLE #the rule is a subtle reference to Conway's game of life
    return RLE

rulestring=int(input("Wolfram rule "))
rule=convertToBinList(rulestring,math.ceil(math.log(rulestring,2)))
print(rule)
cylindricalTape=int(input("Would you like the tape to be cylindrical? (y/n) ")=="y")
bound=int((math.log(len(rule),2)-1)/2)
n=24
loopsAcrossWidths=[]
probabilistic=int(input("Would you like a probabilistic search? (y/n) ")=="y")
if probabilistic==1:
    limit=0
    knownLengths=[]
    while True:
        n=round(random.random()*5)+26
        tape=convertToBinList(round(random.random()*2**n),n)
        subsequentTape=tape
        tapesElapsed=[]
        while tapesElapsed==[] or tapesElapsed.count(subsequentTape)==0 or (limit !=0 and len(tapesElapsed)>=limit):
            tapesElapsed.append(subsequentTape)
            subsequentTape=findSubsequentTape(subsequentTape)
            if len(tapesElapsed)%4096==0:
                print("still going, "+str(len(tapesElapsed))+" iterations simulated, current state: "+str(subsequentTape))
        if len(tapesElapsed)<limit or limit==0:
            loopLength=len(tapesElapsed)-tapesElapsed.index(subsequentTape)
            if knownLengths.count(loopLength)==0:
                knownLengths.append(loopLength)
                print(loopLength,subsequentTape)
else:
    for n in range((bound*2+1)*(cylindricalTape==0),n+1):
        stateTransitions=[]
        for i in range(2**(n-(rulestring==150 and cylindricalTape==0))): #special-case for the rule I'm searching (removes cases where first cell is on to half total computing power because it's its own inverse (I think only satisfied by those that are themselves when flipped tapewise and bitwise))
            tape=convertToBinList(i,n)
            subsequentTape=findSubsequentTape(tape)
            #print(tape,subsequentTape)
            stateTransitions.append(convertToID(subsequentTape))
        #print(stateTransitions)
        print("width "+str(n)+"'s state transitions done")
        loopsPerLength=formatNicely(findLoopsPerLength(stateTransitions))
        loopsAcrossWidths.append(loopsPerLength)
        print(n,loopsAcrossWidths)
        print(graphRLE(loopsAcrossWidths))

    if input("Would you like physics? (y/n) ")=="y":
        size=[800,600]
        black=0,0,0
        screen = pygame.display.set_mode((size[0], size[1]))
        rad=size[0]/len(stateTransitions)
        bitColours=[[int(255*(math.cos((j/n-i/3)*2*math.pi)+1)/2) for i in range(3)] for j in range(n)]
        squares=[]
        for j in range(len(stateTransitions)):
            tapeBits=convertToBinList(j, n)
            squares.append([[[j*rad,0],[size[1]/2,random.random()/2**8]],[rad*2]*2,1, [int(sum([bitColours[k][i]*tapeBits[k] for k in range(n)])/n) for i in range(3)]])
        dims=2
        FPS=60
        drag=0.1
        gravitationalConstant=-10
        hookeStrength=0.001
        def physics():
            for i in range(len(squares)):
                if drag>0:
                    absVel=max(1,math.sqrt(sum([squares[i][0][di][1]**2 for di in range(dims)]))) #each dimension's deceleration from drag is its magnitude as a component of the unit vector of velocity times absolute velocity squared, is actual component times absolute velocity.
                    for di in range(dims):
                        squares[i][0][di][1]*=1-absVel*drag #air resistance
            for i in range(len(squares)-1):
                for i2 in range(i+1,len(squares)):
                    differences=[squares[i2][0][di][0]-squares[i][0][di][0] for di in range(dims)]
                    dist=max(1,math.sqrt(sum([di**2 for di in differences])**3))
                    gravity=gravitationalConstant/dist
                    for di in range(dims):
                        squares[i][0][di][1]+=differences[di]*(hookeStrength*(stateTransitions[i]==i2)+gravity*squares[i2][2])
                        squares[i2][0][di][1]-=differences[di]*(hookeStrength*(stateTransitions[i2]==i)+gravity*squares[i][2])
                for di in range(dims):
                    squares[i][0][di][0]+=squares[i][0][di][1]

        run=True
        while run:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    run = False
            physics()
            screen.fill(black)
            for i in range(len(stateTransitions)):
                pygame.draw.line(screen, 3*(255), [squares[i][0][di][0] for di in range(dims)], [squares[stateTransitions[i]][0][di][0] for di in range(dims)])
            for i in range(len(squares)):
                drawShape(squares[i][1][0],squares[i][1][1],squares[i][0][0][0],squares[i][0][1][0],squares[i][3],1)
            pygame.display.flip()
            clock.tick(FPS)
    else:
        exit()
